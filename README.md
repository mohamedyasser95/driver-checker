# driver-checker
#! /bin/bash 

# Log file path
LOG_FILE="/var/log/driver_checker.log"

# Ensure log file exists
touch $LOG_FILE

# Function to check eligibility
check_eligibility() {
    local name="$1"
    local age="$2"
    local vision_rate="$3"
    local result="not eligible"

    if (( age >= 18 && vision_rate >= 4 )); then
        result="eligible"
    fi

    # Log the result
    echo "$name:$age:$vision_rate:$result" >> $LOG_FILE

    # Output the result to the user
    echo "You are $result for a driver's license."
}

# Function to get user result
get_result() {
    local search_name="$1"
    local result_line

    result_line=$(grep "^$search_name:" $LOG_FILE)

    if [[ -n $result_line ]]; then
        local result=$(echo "$result_line" | awk -F: '{print $4}')
        echo "$search_name:$result"
    else
        echo "No record found for $search_name."
    fi
}

# Function to list all results
list_results() {
    awk -F: '{print $1 ":" $4}' $LOG_FILE
}

# Check if the script received exactly one argument
if [[ $# -ne 1 ]]; then
    echo "Usage: $0 {new|get|list}"
    exit 1
fi

case "$1" in
    "new")
        # Input details for new user
        echo -n "Enter your name: "
        read name
        echo -n "Enter your age: "
        read age
        echo -n "Enter your vision rate (1-6): "
        read vision_rate

        # Validate inputs
        if [[ -z $name || ! $age =~ ^[0-9]+$ || ! $vision_rate =~ ^[1-6]$ ]]; then
            echo "Invalid input. Please try again."
            exit 1
        fi

        # Check eligibility and log the result
        check_eligibility "$name" "$age" "$vision_rate"
        ;;

    "get")
        # Input the name to search
        echo -n "Enter the name to search for: "
        read search_name

        if [[ -z $search_name ]]; then
            echo "Name cannot be empty."
            exit 1
        fi

        # Retrieve and show the result
        get_result "$search_name"
        ;;

    "list")
        # List all user results
        list_results
        ;;

    *)
        echo "Invalid argument. Usage: $0 {new |get | list}"
        exit 1
        ;;
esac#! /bin/bash 

# Log file path
LOG_FILE="/var/log/driver_checker.log"

# Ensure log file exists
touch $LOG_FILE

# Function to check eligibility
check_eligibility() {
    local name="$1"
    local age="$2"
    local vision_rate="$3"
    local result="not eligible"

    if (( age >= 18 && vision_rate >= 4 )); then
        result="eligible"
    fi

    # Log the result
    echo "$name:$age:$vision_rate:$result" >> $LOG_FILE

    # Output the result to the user
    echo "You are $result for a driver's license."
}

# Function to get user result
get_result() {
    local search_name="$1"
    local result_line

    result_line=$(grep "^$search_name:" $LOG_FILE)

    if [[ -n $result_line ]]; then
        local result=$(echo "$result_line" | awk -F: '{print $4}')
        echo "$search_name:$result"
    else
        echo "No record found for $search_name."
    fi
}

# Function to list all results
list_results() {
    awk -F: '{print $1 ":" $4}' $LOG_FILE
}

# Check if the script received exactly one argument
if [[ $# -ne 1 ]]; then
    echo "Usage: $0 {new|get|list}"
    exit 1
fi

case "$1" in
    "new")
        # Input details for new user
        echo -n "Enter your name: "
        read name
        echo -n "Enter your age: "
        read age
        echo -n "Enter your vision rate (1-6): "
        read vision_rate

        # Validate inputs
        if [[ -z $name || ! $age =~ ^[0-9]+$ || ! $vision_rate =~ ^[1-6]$ ]]; then
            echo "Invalid input. Please try again."
            exit 1
        fi

        # Check eligibility and log the result
        check_eligibility "$name" "$age" "$vision_rate"
        ;;

    "get")
        # Input the name to search
        echo -n "Enter the name to search for: "
        read search_name

        if [[ -z $search_name ]]; then
            echo "Name cannot be empty."
            exit 1
        fi

        # Retrieve and show the result
        get_result "$search_name"
        ;;

    "list")
        # List all user results
        list_results
        ;;

    *)
        echo "Invalid argument. Usage: $0 {new |get | list}"
        exit 1
        ;;
esac
