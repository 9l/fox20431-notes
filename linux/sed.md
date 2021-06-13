# sed
stream editor



## 问题
sed file > file is empty.

You can't use the same file for input and output. The shell is destroying your input file by preparing to to write into it. This is done before the sed command even gets a chance to run.

Send the output to a tmpfile or something, then mv tmpfile get_columns.sh
