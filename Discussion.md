Approaches that can be used to solve the problem :-
Read the file in chunks: Since the file is very large, reading it line-by-line or in manageable chunks will avoid memory overload.reading it line by line gives us advantage .
Use Python's built-in file handling: We will use a line-by-line iteration to extract logs for the given date. This will allow us to handle large files without consuming too much memory.Python's built-in file handling is efficient and easy to use when working with large files.
Efficient searching: Each log entry begins with a timestamp in YYYY-MM-DD format, which makes it easy to filter logs for the given date by checking the first 10 characters of each line.
Output to a file: After filtering logs, we will write the results to a new file (output/output_YYYY-MM-DD.txt).
