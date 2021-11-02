# Prob 1
Describe the difference between the varchar and text data types.
Response:
`varchar` defines data types of string up a given length. That is the length is finite.
`text` data types can handle strings of any length.

# Prob 2
Describe the difference between the integer, decimal, and real data types.

Integer: round numbers with no fractional component.

Decimal: numbers with a defined fractional compenent in decimal form. The data type
needs to be defined with two argurments. The first defining how many digits can be
in the overall number and the second being how many digits are to the right
of the decimal point.

Real: numbers defined with any number of decimal digits. 

# Prob 4
What is the largest value that can be stored in an integer column?
Apparently the largest integer in Postgres can be 2147483647

# Prob 5
Describe the difference between the timestamp and date data types.
The timestamp and date data types are different in that the timestamp includes the date
as well as the time appended in the HH:MM:SS:SSS format, where the date type only has
the date in the YYYY-MM-DD format.

# Prob 6
Can a time with a time zone be stored in a column of type timestamp?
No, it only allows for the date.