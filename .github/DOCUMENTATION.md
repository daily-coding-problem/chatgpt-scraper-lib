# ChatGPT Scraper Library Documentation

## Overview

The ChatGPT Scraper Library is a Python package designed to automate interactions with ChatGPT using Selenium. It provides a robust framework for managing browser sessions, handling authentication, and conducting automated conversations with ChatGPT. This library is particularly useful for testing, demonstration purposes, or automated data collection.

### High-Level Architecture

- **Authentication Module**: Manages different methods of logging into ChatGPT, including basic login and 2FA.
- **Browser Module**: Handles the initialization and management of browser sessions using Selenium.
- **Chatbot Module**: Manages chatbot sessions and interactions.
- **Configuration Module**: Centralizes configuration settings and environment variables.
- **Utilities Module**: Provides utility functions for project metadata and other helper functions.
- **Logging Module**: Configures and manages logging across the package.

---

## Module Documentation

### `chatgpt` Package

#### **auth Module**

- **`accounts_deserializer.py`**
	- **Description**: Handles the deserialization of account credentials stored in a base64-encoded JSON structure.
	- **Classes and Functions**:
		- `AccountsDeserializer`: A class responsible for deserializing and providing access to stored account credentials.

- **`login_method.py`**
	- **Description**: Defines different methods for logging into ChatGPT, such as using basic login or OAuth providers.
	- **Classes and Functions**:
		- `LoginMethod`: A class for handling login operations with different providers.

- **`generate_otp.py`**
	- **Description**: Generates One-Time Passwords (OTPs) for two-factor authentication.
	- **Classes and Functions**:
		- `generate_otp`: A function to generate OTPs based on a given secret key.

- **methods Package**
	- **`basic_login.py`**
		- **Description**: Implements the basic login functionality for ChatGPT.
		- **Classes and Functions**:
			- `BasicLogin`: A class that handles the login process using a username and password.

#### **`browser.py`**
- **Description**: Manages browser sessions using Selenium. It handles browser initialization, navigation, and teardown.
- **Classes and Functions**:
	- `Browser`: A class for managing the Selenium browser instance, including options for headless mode and navigation control.

#### **`chatbot.py`**
- **Description**: Handles interactions with the ChatGPT chatbot. This module is responsible for sending prompts and processing responses.
- **Classes and Functions**:
	- `ChatBot`: A class that manages the chat session with ChatGPT, sending prompts, and retrieving responses.

#### **`chatgpt_interaction.py`**
- **Description**: Contains the core logic for interacting with ChatGPT. This includes initializing sessions, handling interactions, and managing the flow of conversations.
- **Classes and Functions**:
	- `ChatGPTInteraction`: A class that wraps the browser and chatbot functionality to provide a streamlined interface for interacting with ChatGPT.

#### **`configuration.py`**
- **Description**: Manages configuration settings and environment variables for the application.
- **Classes and Functions**:
	- `Configuration`: A class that loads and provides access to various configuration options.

#### **`logger_config.py`**
- **Description**: Configures and manages logging for the entire package.
- **Classes and Functions**:
	- `setup_logging`: A function to initialize logging based on the configuration.

#### **`temporary_chat.py`**
- **Description**: Manages temporary chat sessions with ChatGPT, where interactions are not stored in history.
- **Classes and Functions**:
	- `TemporaryChat`: A class that toggles temporary chat mode and manages related configurations.

### `utilities` Package

- **`poetry.py`**
	- **Description**: Provides utility functions for fetching project metadata such as the project's name, version, and author details.
	- **Functions**:
		- `get_name`: Returns the project name.
		- `get_version`: Returns the project version.
		- `get_description`: Returns the project description.
		- `get_authors`: Returns the project authors.

- **`__init__.py`**
	- **Description**: Initializes the `utilities` module.

---

Here’s how you can integrate the `TEST_ACCOUNTS` configuration process into the documentation while making it agnostic to environment variables:

---

## Usage Examples

### Basic Usage

```python
from chatgpt.browser import Browser
from chatgpt.chatgpt_interaction import ChatGPTInteraction
from chatgpt.configuration import Configuration

# Setup configuration
config = Configuration(headless=True, use_temporary_chat=True)

# Initialize browser
browser = Browser("https://chatgpt.com", config.headless)

# Initialize interaction session
interaction = ChatGPTInteraction(browser, config)

# Perform login if necessary
interaction.login("your-email@example.com", "your-password")

# Send a prompt and get a response
response = interaction.chat("What is the weather today?")
print(response)

# End the session
browser.quit()
```

### Advanced Usage with Authentication

If you need to authenticate multiple accounts or use specific credentials securely, you can use the Accounts Serializer tool to generate and manage these credentials and pass them to the ChatGPT scraper via an environmental variable or some other secure method.

#### Configuring Multiple Accounts with 2FA

To securely store and pass credentials for test accounts to the ChatGPT scraper, you can generate a base64-encoded JSON structure using the [Accounts Serializer](https://github.com/daily-coding-problem/accounts-serializer) tool.

**Steps to Configure Multiple Accounts with 2FA:**

1. **Clone the Accounts Serializer Repository**

   Begin by cloning the repository to access the tool:

   ```sh
   git clone https://github.com/daily-coding-problem/accounts-serializer.git
   cd accounts-serializer
   ```

2. **Install Dependencies**

   Ensure you have Python 3.11 or higher and Poetry installed, then install the dependencies:

   ```sh
   poetry install
   ```

3. **Generate the JSON Structure**

   Run the `accounts_serializer.py` script with your account details to create the JSON structure:

   ```sh
   poetry run python accounts_serializer.py \
       --emails test@company.com user@anothercompany.com \
       --passwords password123 userpassword456 \
       --providers basic google \
       --secrets google:google-secret-abc chatgpt:chatgpt-secret-xyz github:github-secret-123 aws:aws-secret-789
   ```

   This will generate a JSON structure similar to the following:

   ```json
   {
       "test@company.com": {
           "provider": "basic",
           "password": "password123",
           "secret": {
               "google": "google-secret-abc",
               "chatgpt": "chatgpt-secret-xyz"
           }
       },
       "user@anothercompany.com": {
           "provider": "google",
           "password": "userpassword456",
           "secret": {
               "github": "github-secret-123",
               "aws": "aws-secret-789"
           }
       }
   }
   ```

4. **Base64 Encode the JSON Structure**

   To secure the credentials, encode the JSON structure into a base64 string:

   ```sh
   echo -n '{"test@company.com": {"provider": "basic", "password": "password123", "secret": {"google": "google-secret-abc", "chatgpt": "chatgpt-secret-xyz"}}, "user@anothercompany.com": {"provider": "basic", "password": "userpassword456", "secret": {"github": "github-secret-123", "aws": "aws-secret-789"}}}' | base64
   ```

5. **Using the Encoded Credentials**

   The base64-encoded string can be securely passed to your application or stored in a configuration file.

   Here's an example of how to use these credentials in your project:

   ```python
   from chatgpt.auth.accounts_deserializer import AccountsDeserializer
   from chatgpt.configuration import Configuration

   # Initialize the AccountsDeserializer with the base64 encoded string
   encoded_accounts = "base64_encoded_json_structure"
   accounts = AccountsDeserializer(encoded_accounts)

   # Setup configuration
   config = Configuration(accounts=accounts.get_all_accounts(), headless=True)

   # Proceed with interactions as shown in the basic usage example
   ```

   This allows you to securely manage and utilize multiple accounts within your application.

---

## API Reference

### Classes

- **`AccountsDeserializer`**:
	- **Methods**: `get_all_accounts()`

- **`LoginMethod`**:
	- **Methods**: `derive_login_provider()`

- **`Browser`**:
	- **Methods**: `__init__()`, `navigate()`, `quit()`

- **`ChatBot`**:
	- **Methods**: `chat()`, `send_prompt()`, `get_response()`

- **`ChatGPTInteraction`**:
	- **Methods**: `login()`, `chat()`, `end_session()`

- **`Configuration`**:
	- **Methods**: `__init__()`, `load_env()`

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.
