Approaches that can be used to solve the problem :-
Read the file in chunks: Since the file is very large, reading it line-by-line or in manageable chunks will avoid memory overload.reading it line by line gives us advantage .
Use Python's built-in file handling: We will use a line-by-line iteration to extract logs for the given date. This will allow us to handle large files without consuming too much memory.Python's built-in file handling is efficient and easy to use when working with large files.
Efficient searching: Each log entry begins with a timestamp in YYYY-MM-DD format, which makes it easy to filter logs for the given date by checking the first 10 characters of each line.
Output to a file: After filtering logs, we will write the results to a new file (output/output_YYYY-MM-DD.txt).
Explanation of code 
importing Required Modules
Copy
import mmap
import bisect
mmap: Provides a way to map the file into memory for efficient access.
bisect: This is imported but not used in the code. Normally used for binary searching in sorted lists.
Function Definition: find_start_offset

This function takes file_path and target_time (the timestamp you're looking for) as inputs and returns the offset of the first log entry with a timestamp greater than or equal to target_time
Opening the File Using Memory Mapping
The file at file_path is opened in binary read mode ('rb').
The mmap object (mmapped_file) maps the entire file into memory, enabling faster and more memory-efficient file access.
Initialize Search Range

python
Copy
start, end = 0, len(mmapped_file)
We initialize the start (0) and end positions (len(mmapped_file)) for our binary search.
Binary Search Loop

python
Copy
while start < end:
    mid = (start + end) // 2
The search loop performs a binary search, adjusting the start and end positions in the file.
Navigate to the Start of a Log Line

python
Copy
while mid > 0 and mmapped_file[mid] != ord('\n'):
    mid -= 1
if mid > 0:
    mid += 1
The loop moves mid backwards to find the beginning of the current log line by searching for a newline (\n).
This ensures that the current position (mid) is at the start of a log line.
Extract the Log Line

python
Copy
line_start = mid
line_end = mmapped_file.find(b'\n', mid)
if line_end == -1:
    break
line_start marks where the current line starts, and line_end is where the next newline character is found, indicating the end of the line.
If no newline is found (line_end == -1), the loop breaks, indicating we reached the end of the file.
Decode and Parse the Log Line

python
Copy
line = mmapped_file[line_start:line_end].decode()
parts = line.split(" ", 2)
The line is extracted from mmapped_file between line_start and line_end, then decoded from bytes to a string.
The line is split into three parts: timestamp, log level, and message.
Timestamp Comparison

python
Copy
if len(parts) < 3:
    start = mid + 1
    continue
timestamp = parts[0]

if timestamp >= target_time:
    return mid  # Found the approximate position
If the log entry has fewer than 3 parts, we continue searching.
The timestamp (parts[0]) is compared with the target_time. If it's greater than or equal to the target, the function returns the current offset (mid), indicating where the log entry starts.
Adjust Search Range

python
Copy
start = line_end + 1
If the timestamp is less than the target_time, the search continues from the next log line (line_end + 1).
Return None (If Not Found)

python
Copy
return None  # Timestamp not found
If no matching log entry is found, the function returns None.

Running of code
Input: You provide a file path (file_path) and a target timestamp (start_time) to the function find_start_offset.

Example:

python
Copy
file_path = r"C:\Users\HP\Downloads\logs_2024.log\logs_2024.log"
start_time = "2024-01-01T00:00:00"
Step 1: Open the File and Map it into Memory

The file is opened in binary mode ('rb'), and the contents are mapped into memory.
Step 2: Initialize Search Range

The search range is set to the entire file (start = 0, end = len(mmapped_file)).
Step 3: Binary Search

The code enters a while loop, continuously adjusting the search range (start and end) using binary search.
Step 4: Find Log Line Boundaries

For each middle point (mid) in the search range, the code finds the start and end of the log line by searching for newline characters.
Step 5: Extract and Parse the Log Line

The code extracts and decodes the log line, splitting it into parts (timestamp, log level, message).
Step 6: Compare Timestamp

The timestamp of the log entry is compared with the target_time.
Step 7: Adjust Search Range

If the timestamp is less than the target_time, the search continues after the current log entry. If the timestamp is greater than or equal to the target_time, the current position is returned as the offset.
