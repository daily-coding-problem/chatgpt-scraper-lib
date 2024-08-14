# ChatGPT Scraper Library

![Linux](https://img.shields.io/badge/-Linux-FCC624?style=flat-square&logo=linux&logoColor=black)
![Python](https://img.shields.io/badge/-Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Selenium](https://img.shields.io/badge/-Selenium-59b943?style=flat-square&logo=selenium&logoColor=white)

A Selenium-based ChatGPT interaction automation library. This library allows you to interact with ChatGPT using predefined prompts, automating conversations and fetching responses for testing or demonstration purposes. It can be easily integrated into any CLI application.

## Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Running Tests](#running-tests)
- [Learn More](#learn-more)
- [License](#license)

## Features

- Utilizes Selenium to automate and scrape ChatGPT conversations.
- Supports fetching responses for predefined prompts.
- Allows integration with multiple login methods (Basic and Google).
- Supports 2FA for secure login methods.
- Provides mechanisms to copy ChatGPT responses in Markdown or Plain Text format.
- Allows configuration of headless mode and temporary chat mode.

## Prerequisites

Before you begin, ensure you have met the following requirements:

- Python 3.12 or higher.

## Installation

**Clone the Repository**

```sh
git clone https://github.com/daily-coding-problem/chatgpt-scraper-lib.git
cd chatgpt-scraper-lib
```

**Setup Python Environment**

Use the following commands to set up the Python environment:

```sh
python -m venv .venv
source .venv/bin/activate
pip install poetry
poetry install --no-root
```

## Running Tests

Run the tests with the following command:

```sh
poetry run pytest
```

## Learn More

For more comprehensive details about the library's architecture, module usage, API reference, and advanced examples, please refer to the [DOCUMENTATION.md](.github/DOCUMENTATION.md) file in this repository.

## License

This project is licensed under the MIT License - see the [LICENSE](.github/LICENSE) file for details.
