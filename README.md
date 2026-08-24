# Codex Vivaldi Plugin

A small community plugin that lets Codex use the official ChatGPT browser
extension from Vivaldi, including installations where the connection is
reported as Google Chrome.

## How it works

The plugin contains only original marketplace metadata and skill instructions.
It does not bundle, copy, patch, or redistribute OpenAI's extension, native
host, browser runtime, documentation, or assets. At runtime it uses the
official Chrome browser runtime already installed by Codex on the user's
computer.

## Requirements

- Codex desktop with the official Chrome browser plugin installed
- The official ChatGPT Chrome extension installed and enabled in Vivaldi
- Vivaldi open and connected to Codex

## Install

Clone this repository, add it as a local marketplace, and install the plugin:

```sh
git clone https://github.com/hangriver/codex-vivaldi-plugin.git
codex plugin marketplace add ./codex-vivaldi-plugin
codex plugin add vivaldi@vivaldi-community
```

Start a new Codex task after installation so the skill is loaded.

## Safety behavior

The plugin tries a native Vivaldi connection first. If Codex exposes Vivaldi
as a single Chrome-family extension connection, it verifies the user's stated
tab context before using it. Ambiguous or multiple extension connections fail
closed and require user confirmation.

## License and trademarks

The files in this repository are licensed under MIT. OpenAI, ChatGPT, Codex,
Chrome, and Vivaldi are trademarks of their respective owners. This project is
independent, is not affiliated with or endorsed by OpenAI or Vivaldi, and does
not grant rights to any third-party software or trademark.
