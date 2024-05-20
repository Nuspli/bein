# bein - better input

## About

Tool for parsing specific escape sequences and providing them as input in place to a process.
Useful for purposes where raw bytes need to be sent to a process as input i.e. binary exploitation.
The goal here is not to replace exploit scripts by any means, but to offer a quick and easy way to
test input to a process without having to touch a code editor.

## Usage

```txt
python bein.py [-h] [-o <file>] [-v] [-a] [-an] [-l] [-n] [-p <process>]
```

## Options

```txt
  -h           : Show this help message and exit
  -o <file>    : Write output to file instead of stdout
  -v           : Be verbose, prints all exceptions and errors
  -a           : Only output standard keyboard characters (ascii 32-127)
                 Anything else will be written as [0x??]
  -an          : Like -a except allow newline characters too
  -l           : Use little endian byte order when parsing hex values initialized with \0x
  -n           : Strip newline from input
  -p <process> : Run the specified process and parse input before sending it to the process
                 To pass additional arguments to the process itself,
                 use quotes around the command: -p 'ls -al'
                 Alternatively, use: -p bash or: -p sh and run commands from the shell
```

## Requirements

```txt
prompt_toolkit
pwntools
```

## Base Features

```txt
\x41          -> A
\x41\x42      -> AB
\x413         -> A3
\xGG          -> \xGG

\0x41         -> A
\0x4142       -> AB
                 be careful with putting stuff after the hex value.
                 if you need to add more characters after \0x4142,
                 use a \ to terminate the sequence
\0x4142abc    -> [0x04 0x14 0x2a 0xbc] likely not what you want
\0x4142\abc   -> ABabc

(using little endian option: -l only affects \0x sequences)
\0x41\0x42    -> AB
\0x4142       -> BA
\0x4142\abc   -> BAabc

\0            -> null byte
\n            -> newline
\r            -> carriage return
```

This tool heavily depends on pwntools ability to handle I/O of processes and prompt toolkit
for a nicer command line experience, allowing you to use the arrow keys like in bash and
auto completions based on your input history.

> I will keep updating this project as I see fit. Suggestions are welcome.
