# Finds ipv4 addresses in log files / stdout 

## Tested against Windows 10 / Python 3.10 / Anaconda

### pip install iptoolv4

The module iptoolv4 is designed for working with IPv4 addresses and IP-related data in various scenarios, such as analyzing log files or subprocess outputs to identify and manipulate IPv4 addresses. Below are some key points regarding its purpose, potential users, and advantages:

Purpose:

### IP Address Handling: 

The module provides methods to load, save, and search for IPv4 addresses in text data, 
as well as to observe log files and subprocess output for IP-related information.

### Custom Formatting: 

It allows users to define custom formatting for found IPv4 addresses, 
providing flexibility in how IP addresses are displayed or processed.

### IP Address Filtering: 

Users can filter out specific IP addresses.

### Concurrent Processing: 

The module supports concurrent searching for IP addresses, 
which can be advantageous for handling large volumes of data efficiently.


## Advantages

### Customization: 

Users can define their own formatting and processing logic for 
found IPv4 addresses, making it adaptable to various use cases.

### Concurrent Processing: 

The module supports concurrent processing, 
enhancing efficiency when dealing with large datasets or real-time monitoring.

### Hide My IP: 

The option to hide specific IP addresses (e.g., localhost) 
can simplify analysis by filtering out irrelevant data.

### DataFrame Integration: 

The use of Pandas DataFrames for storing IP address ranges and search 
results makes it easy to work with and analyze data.

### Versatility: 

It can be used for a wide range of IP address-related tasks, 
including monitoring network activity, analyzing logs, and preprocessing data.



```python
import subprocess
from cprinter import TC
from iptoolv4 import IpV4Tool

chunks = 10000000
concurrent = True
self = IpV4Tool(
    hide_my_ip=True
)  # 127.0.0.1 and this PC's IP address will not be in the results
# IMPORTANT: aa_startip == start of ip range  /  aa_endip == end of ip range
# using a arbitrary free database from https://lite.ip2location.com/database/ip-country-region-city
# The format of the database takes a while (when using a csv), so be patient, but it has to be done only once
self.load_wanted_ips(r"C:\citylight400mb.pkl", start="aa_startip", end="aa_endip")
# After the database is ready, you can save the pandas DataFrame to a file and load it later:
self.save_wanted_ips(r"C:\citylight400mb.pkl")
# without color formating
df = self.search_for_ip_addresses(
    path_string_bytes=R"C:\newsfxaqq.txt",
    chunks=1000000,
    concurrent=True,
    substitute=False,
)
# an example for color formating
subsfu = (
    subsi
) = lambda j, i: f""" {TC('>>>').fg_green.bg_black.text} {TC(str(j['aa_city'].iloc[0])).fg_red.bg_black.text}: {i.decode('utf-8','replace')} {TC('>>>').fg_green.bg_black.text} """.encode(
    "utf-8", "replace"
)
df = self.search_for_ip_addresses(
    path_string_bytes=R"C:\newsfxaqq.txt",
    chunks=1000000,
    concurrent=True,
    substitute=subsfu,
)
print(df[-1].decode("utf-8"))

# an example for observing a log file generated by another process
newfile = r"c:\newlogfile.txt"
subprocess.Popen("netstat -b 5 > " + newfile + "", shell=True)
self.observe_log_file(
    newfile,
    min_len=5,
    columns=(
        "aa_country",
        "aa_city",
        "aa_destrict",
        "aa_lati",
        "aa_longi",
    ),
    print_df=False,
    omit_columns=False,
    chunks=chunks,
    concurrent=concurrent,
)

# an example for observing a subprocess (stdout)
self.observe_subprocess(
    cmdline="tracert -4 google.com",
    min_len=5,
    columns=(
        "aa_country",
        "aa_city",
        "aa_destrict",
        "aa_lati",
        "aa_longi",
    ),
    print_df=False,
    shell=True,
    omit_columns=False,
    chunks=chunks,
    concurrent=concurrent,
)

# an example for observing a subprocess (stdout)
self.observe_subprocess(
    cmdline="netstat -b 6",
    min_len=5,
    columns=(
        "aa_country",
        "aa_city",
        "aa_destrict",
        "aa_lati",
        "aa_longi",
    ),
    print_df=False,
    shell=True,
    omit_columns=False,
    chunks=chunks,
    concurrent=concurrent,
)

```