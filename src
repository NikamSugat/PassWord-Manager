#!/bin/bash

# Password Manager Script with Enhanced Banner

# Banner function with colorful and highlighted text
display_banner() {
    echo -e "\e[1;34m
    ╔══════════════════════════════════╗
    ║   Password Manager                ║
    ║  \e[1;32mMade By Sugat Nikam\e[1;34m          ║
    ║                                    ║
    ║  Gmail: \e[1;36mgamersugat571@gmail.com\e[1;34m  ║
    ║  Linkedin: \e[1;36mwww.linkedin.com/in/nikamsugat\e[1;34m ║
    ║  Try Hackme: \e[1;36mhttps://tryhackme.com/p/Sugat\e[1;34m  ║
    ║  Badge: \e[1;36mhttps://tryhackme.com/badge/2065293\e[1;34m ║
    ╚══════════════════════════════════╝
    \e[0m"
}

# Check if the passwords file exists, if not, create it
PASSWORD_FILE="passwords.txt"
touch "$PASSWORD_FILE"

# Function to securely generate a random password with given length and complexity
generate_password() {
    local length=$1
    local uppercase=$2
    local lowercase=$3
    local number=$4
    local signs=$5
    local chars=""

    [[ $uppercase == "y" ]] && chars+="A-Z"
    [[ $lowercase == "y" ]] && chars+="a-z"
    [[ $number == "y" ]] && chars+="0-9"
    [[ $signs == "y" ]] && chars+="!@#\$%^&*()_-+=<>?"

    # Use a temporary variable to avoid the issue with '_-+' in reverse collating sequence order
    local temp_chars=$(echo "$chars" | sed 's/\(.\)/[\1]/g')

    tr -dc "$temp_chars" </dev/urandom | head -c "$length" ; echo ''
}

# Function to add a new password
add_password() {
    echo "Enter the name for the password:"
    read -r name

    # Check if the password for the given name already exists
    grep -q "^$name:" "$PASSWORD_FILE" && {
        echo "Password for '$name' already exists. Choose a different name."
        return 1
    }

    echo "Enter the length of the password:"
    read -r length

    echo "Include uppercase? (y/n):"
    read -r uppercase

    echo "Include lowercase? (y/n):"
    read -r lowercase

    echo "Include numbers? (y/n):"
    read -r number

    echo "Include signs? (y/n):"
    read -r signs

    password=$(generate_password "$length" "$uppercase" "$lowercase" "$number" "$signs")

    echo "$name:$password" >> "$PASSWORD_FILE"
    echo "Password added successfully for '$name'."
}

# Function to retrieve a password
get_password() {
    echo "Enter the name for the password you want to retrieve:"
    read -r name

    # Retrieve the password for the given name
    password=$(grep "^$name:" "$PASSWORD_FILE" | cut -d':' -f2)

    if [ -n "$password" ]; then
        echo "Password for '$name': $password"
    else
        echo "Password not found for '$name'."
    fi
}

# Function to open a URL in the default web browser
open_url() {
    echo -e "\nChoose a URL to open:"
    echo "1. LinkedIn"
    echo "2. Gmail"
    echo "3. TryHackMe"
    echo "4. TryHackMe Badge"
    read -p "Enter the corresponding number (1-4): " choice

    case $choice in
        1) url="https://www.linkedin.com/in/nikamsugat" ;;
        2) url="mailto:gamersugat571@gmail.com" ;;
        3) url="https://tryhackme.com/p/Sugat" ;;
        4) url="https://tryhackme.com/badge/2065293" ;;
        *) echo "Invalid option." ; return ;;
    esac

    # Try to open the URL based on the platform
    if command -v xdg-open &> /dev/null; then
        xdg-open "$url"  # Linux
    elif command -v open &> /dev/null; then
        open "$url"  # macOS
    elif command -v start &> /dev/null; then
        start "$url"  # Windows
    else
        echo "Unable to open the URL. Please open it manually in your web browser."
    fi
}

# Function to display "Made by Sugat Nikam" with blinking colors
made_by() {
    echo -e "\e[5m\e[1;31mMade by Sugat Nikam\e[0m"
}

# Display the banner
display_banner

# Main menu
while true; do
    echo -e "\nPassword Manager Menu:"
    echo "1. Add a new password"
    echo "2. Retrieve a password"
    echo "3. Open URL"
    echo "4. Exit"
    read -p "Choose an option (1/2/3/4): " choice

    case $choice in
        1) add_password ;;
        2) get_password ;;
        3) open_url ;;
        4) made_by; echo "Exiting Password Manager."; break ;;
        *) echo "Invalid option. Please choose 1, 2, 3, or 4." ;;
    esac
done
