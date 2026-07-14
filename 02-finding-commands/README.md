# Finding Commands When You Don't Know the Exact Command

## Searching When You Don't Know the Exact Command

### The Scenario

You know you want to do something network-related, but you're not sure which command to use.

### The Solution — `apropos`

The `apropos` command searches through manual page descriptions and helps you find commands related to a specific keyword.

```shell
apropos network
```

This command searches for the keyword `network` in command descriptions and displays commands that are related to networking.

---

## Getting More Specific

Sometimes `apropos` returns too many results. Here are some ways to narrow it down:

### Show only exact matches

Only show exact matches for `zip`:

```shell
apropos -e zip
```

---

### Show only commands from Section 1

Section `1` contains user commands and executable programs.

```shell
apropos -s 1 network
```

---

### Use patterns with regular expressions

Show everything that starts with `git`:

```shell
apropos -r '^git.*'
```

This is useful when you know the beginning of a command name but are not sure about the complete command.
