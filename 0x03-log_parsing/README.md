# 0x03. Log Parsing
Here's an overview of the key concepts needed for your "0x03. Log Parsing" project:

### 1. **File I/O in Python**

-   **Reading from `sys.stdin`**: This allows your program to read input directly from the console or a data stream. It's useful when processing logs or real-time data.
    -   Example:
		```
		import sys
		for line in sys.stdin:
		    print(line.strip())  # Strip removes extra whitespace or newline characters
		```
	-	**Resource**: [Python Input and Output](https://docs.python.org/3/tutorial/inputoutput.html)
### 2. **Signal Handling in Python**

-   **Handling Keyboard Interruptions**: When processing data in real-time, users may stop the program (e.g., by pressing `CTRL + C`). Signal handling ensures the program can handle such interruptions gracefully.
    -   Example
		```
		import signal
		import sys

		def signal_handler(sig, frame):
		    print('You pressed Ctrl+C! Exiting...')
		    sys.exit(0)

		signal.signal(signal.SIGINT, signal_handler)

		while True:
		    pass  # Simulating an infinite loop
		```
	-	**Resource**: [Python Signal Handling](https://docs.python.org/3/library/signal.html)
### 3. **Data Processing**

-   **Parsing Strings**: Extracting relevant data from logs often involves string parsing, where you split or search for patterns within a log line.
    -   Example
		```
		log = "127.0.0.1 - - [01/Jan/2022] \"GET /index.html HTTP/1.1\" 200 1234"
		parts = log.split()
		status_code = parts[-2]  # Extracts the status code
		file_size = parts[-1]    # Extracts the file size
		```
	- **Aggregating Data**: This involves summing file sizes or counting occurrences of status codes to generate summaries like totals or averages.
### 4. **Regular Expressions**

-   Regular expressions (regex) are useful for validating and extracting specific data formats (like IP addresses or timestamps) from logs.
    -   Example
		```
		import re

		log = '127.0.0.1 - - [01/Jan/2022] "GET /index.html HTTP/1.1" 200 1234'
		match = re.match(r'(\d{1,3}\.){3}\d{1,3}', log)  # Matches the IP address
		if match:
		    print(match.group(0))
		```
	- **Resource**: [Python Regular Expressions](https://docs.python.org/3/library/re.html)

### 5. **Dictionaries in Python**

-   Dictionaries are excellent for counting occurrences, such as the number of each HTTP status code in your logs.
    -   Example
		```
		counts = {}
		status_code = "200"
		counts[status_code] = counts.get(status_code, 0) + 1  # Increment the count for the status code
		```
	- **Resource**: [Python Dictionaries](https://docs.python.org/3/tutorial/datastructures.html#dictionaries)
### 6. **Exception Handling**

-   Handling potential errors (like a malformed log line or division by zero when calculating averages) using `try` and `except`.
    -   Example
		```
		try:
		    result = 10 / 0
		except ZeroDivisionError:
		    print("Cannot divide by zero!")
		```
	- **Resource**: [Python Exceptions](https://docs.python.org/3/tutorial/errors.html)

## Example
Let's go through a basic example where we process log data from `sys.stdin`, count occurrences of HTTP status codes, and accumulate the total file size.

### Problem:

We need to process a stream of log entries like the one below:
```
127.0.0.1 - - [01/Jan/2022] "GET /index.html HTTP/1.1" 200 1234
```
Each line contains:

1.  **IP Address**: `127.0.0.1`
2.  **HTTP Method and URL**: `"GET /index.html HTTP/1.1"`
3.  **HTTP Status Code**: `200`
4.  **File Size**: `1234` bytes

We will:

-   Count how many times each status code appears.
-   Keep a running total of the file sizes.

### Steps:

1.  **Reading from `sys.stdin`**.
2.  **Parsing the log line** to extract the status code and file size.
3.  **Counting status codes** using a dictionary.
4.  **Summing file sizes**.
5.  **Handling keyboard interruption (`CTRL + C`)** to gracefully exit and print the final summary.

### Example Code:
```
import sys
import signal

# Initialize dictionary to count status codes and a variable for total file size
status_counts = {}
total_file_size = 0

def signal_handler(sig, frame):
    """Handles keyboard interruption (CTRL + C) and prints summary."""
    print("\nSummary:")
    print(f"Total file size: {total_file_size}")
    for status_code, count in status_counts.items():
        print(f"Status code {status_code}: {count} times")
    sys.exit(0)

# Register signal handler for keyboard interruption
signal.signal(signal.SIGINT, signal_handler)

def process_log_line(line):
    """Parses a single log line, updates status counts and total file size."""
    global total_file_size

    # Split the log line by spaces (basic parsing)
    parts = line.split()

    if len(parts) < 2:
        return  # Handle malformed log line

    # Extract status code and file size from the appropriate parts of the log line
    status_code = parts[-2]  # Second last element is the status code
    file_size = parts[-1]    # Last element is the file size
    
    try:
        # Convert file size to an integer
        file_size = int(file_size)
    except ValueError:
        # If file size is not an integer, skip the line
        return

    # Update the status code count in the dictionary
    if status_code not in status_counts:
        status_counts[status_code] = 0
    status_counts[status_code] += 1

    # Add the file size to the total
    total_file_size += file_size

# Main loop: read from stdin and process each line
print("Processing logs... (press CTRL + C to stop and see the summary)")

for line in sys.stdin:
    process_log_line(line.strip())
```
### Explanation:

1.  **Signal Handling**:
    
    -   The `signal_handler` function is triggered when the user presses `CTRL + C`. It prints a summary of the status counts and total file size before exiting.
2.  **Log Parsing**:
    
    -   The log line is split by spaces, and the status code is extracted from the second-to-last part, while the file size is taken from the last part.
3.  **Exception Handling**:
    
    -   We use a `try` block to handle cases where the file size isn't a valid integer (e.g., malformed logs).
4.  **Status Code Counting**:
    
    -   We use a dictionary to count the occurrences of each status code. If the status code is not already in the dictionary, we initialize its count to 0.
5.  **File Size Summing**:
    
    -   We accumulate the total file size by adding each log's file size to `total_file_size`.

### How to run:

-   Save the script to a file (e.g., `log_parser.py`).
-   Pipe log data into the script using the terminal, for example:
	```
	cat log_file.txt | python3 log_parser.py
	```
- Alternatively, you can manually input lines and hit `CTRL + C` to stop.
### Example Input:
```
127.0.0.1 - - [01/Jan/2022] "GET /index.html HTTP/1.1" 200 1234
127.0.0.1 - - [01/Jan/2022] "POST /form HTTP/1.1" 404 567
```
### Output (on `CTRL + C`):
```
Processing logs... (press CTRL + C to stop and see the summary)
^C
Summary:
Total file size: 1801
Status code 200: 1 times
Status code 404: 1 times

```
