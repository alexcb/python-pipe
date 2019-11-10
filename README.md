# python-pipe
python-pipe is a command line tool for processing text files line-by-line; it's like awk but for python-users.

# Usage

    cat <input file> | python-pipe [--start <INIT_CODE>] <CODE> [--end <FINAL_CODE>]
    
`<CODE>` is a single line of python code which will be executed; it will have a variable `l` or `line` which is set to the current line being processed. `<INIT_CODE>` and `<FINAL_CODE>` will be run at the start and end of the program respectively. If `<FINAL_CODE>` is ommitted, any global variables (aside from the line) will be displayed. 

# Examples

## Input file

    $ cat sushi
    Sake             3.95
    Maguro           4.50
    Ebi              3.95
    Tai              4.80
    Tako             5.00
    Inari            3.25

## transform into CSV (line only transform)
    $ cat sushi | python-pipe 'print(",".join(l.split()))'
    Sake,3.95
    Maguro,4.50
    Ebi,3.95
    Tai,4.80
    Tako,5.00
    Inari,3.25

## compute sums (example initilizating variables via --start)
    $ cat sushi | python-pipe --start 'total=0; num=0' 'total += float(l.split()[-1]); num += 1'
    total: 25.45
    num: 6

## compute average (example using the --end option to override the default behaviour of printing all variables)
    $ cat sushi | python-pipe --start 'total=0; num=0' 'total += float(l.split()[-1]); num += 1' --end 'print(f"average: {total/num}")'
    average: 4.241666666666666
