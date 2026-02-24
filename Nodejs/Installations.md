# NodeJS Installation

## 1. Download NodeJS

- Go to the official NodeJS website: https://nodejs.org/en/download
- Download the LTS (Long Term Support) version for your operating system
- Run the installer and follow the instructions

## 2. Windows Installation

- Open a terminal or command prompt
- Run the following commands:
  ```bash
    # Download and install Chocolatey:
    powershell -c "irm https://community.chocolatey.org/install.ps1|iex"

    # Download and install Node.js:
    choco install nodejs --version="24.13.1"

    # Verify the Node.js version:
    node -v # Should print "v24.13.1".

    # Verify npm version:
    npm -v # Should print "11.8.0".
  ```
- You should see the version numbers printed to the console

## 3. Linux Installation

- Open a terminal or command prompt
- Run the following commands:
  ```bash
    # Download and install Chocolatey:
    sudo apt-get update
    sudo apt-get install -y curl
    curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
    sudo apt-get install -y nodejs

    # Verify the Node.js version:
    node -v # Should print "v24.13.1".

    # Verify npm version:
    npm -v # Should print "11.8.0".
  ```
- You should see the version numbers printed to the console

## 4. MacOS Installation

- Open a terminal or command prompt
- Run the following commands:
  ```bash
    # Download and install Homebrew:
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

    # Download and install Node.js:
    brew install node@24

    # Verify the Node.js version:
    node -v # Should print "v24.13.1".

    # Verify npm version:
    npm -v # Should print "11.8.0".
  ```
- You should see the version numbers printed to the console

---
---