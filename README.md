# ft_irc

A functional IRC server written in C++98, built from the ground up using only POSIX sockets and the standard library.

Clients connect with any standard IRC client (tested with [irssi](https://irssi.org/) and [WeeChat](https://weechat.org/)) and interact through the standard IRC protocol.

## Features

- Handles multiple simultaneous clients via a single `select()` loop — no threads, no blocking I/O
- Full registration flow: `PASS`, `NICK`, `USER` with validation and IRC-compliant error replies
- Channel management: `JOIN` with optional password, client limit enforcement
- Channel operators: first client to join becomes operator, `MODE` checks privileges
- Connection keepalive: `PING` / `PONG`
- Clean disconnection: `QUIT` closes socket, removes client from all data structures
- RFC-compliant numeric replies (001 welcome, 353 NAMREPLY, 461 NEEDMOREPARAMS, etc.)
- `SIGINT` handled gracefully — server closes socket and exits cleanly

## Architecture

| File | Role |
|---|---|
| `server.cpp` | Core loop, socket setup, request parsing, command dispatch |
| `command.cpp` | IRC command implementations (PASS, NICK, USER, JOIN, MODE, QUIT, PING) |
| `client.cpp` | Client object: stores nickname, username, hostname, socket fd |
| `channel.cpp` | Channel object: client list, password, operator map |
| `ft_split.cpp` | C string splitting utility (from libft, adapted for C++) |

## Key concepts demonstrated

- Non-blocking I/O multiplexing with `select()` and `fd_set`
- TCP socket lifecycle: `socket`, `bind`, `listen`, `accept`, `send`, `recv`, `close`
- IRC protocol parsing: raw byte stream → command map → handler dispatch
- Object-oriented design in C++98 (no smart pointers, manual memory management)
- Signal handling with `sigaction` for clean shutdown on `SIGINT`

## Build & run

```bash
make
./irc <port> <password>
```

```bash
./irc 6667 0000
```

Then connect with any IRC client:

```bash
irssi -c localhost -p 6667 -w 0000
```

## Project info

| | |
|---|---|
| Language | C++98 |
| Compiler | c++ -Wall -Wextra -Werror -std=c++98 |
| School | École 42 Paris |
