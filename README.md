# linux
The linux readme contains all the commands

## What is linux?
    Linux is a operating system kernal.
    The kernal is a bridge between your application and hardware.

    When django application does
        with open(app.py) as f:
            print(f.read())
    
    Python doesnot read the disk directly.
    
    python -> kernal -> disk

    Kernal performs the actul disk operation and returns data to python.

    The same is true for:
        reading/writing files
        opening sockets
        allocating memory
        creating processes
        communicating over network
    The kernal manages all of the resources

### Why do backend developers need linux?
    Suppose the django application deployed on ubuntu server
    we need to
        ssh into the system -> check if application is running or not -> view logs -> restart services -> Monitor cpu usage -> check memory usage -> debug networking issues -> deploy new versions
    All this happens in linux

    Unlike windows (C:\, D:\), linux has one unified directory tree.
    Everthing starts with /

### File system and uses
    / - root directory

    /home - contains all user files
    /home/koushik - this where the documents, downloads, projects usually stay
    /root - home directory of the root(administrator) user.

    Is / as same /root?
        / = top level root directory
        /root = home directory of the root user
    
    /etc - Contains system configuration files
    Example:
        ssh files
        hosts
        passwd
        fstab
    when configuring services like ngnix or ssh, you will often edit files under /etc

    /var - stores data that changes frequently
    Example:
        logs
        Database files
        Cache
        Mail
        Application runtime data
    If your django application crashes you may need to see logs at /var/logs

    /tmp - Temporary files, Applications often use this directory for short lived data,Files here may be deleted automatically or by cleanup process

    /bin - Contains essential user commands
    Example:
        ls, cp, mv, cat
    
    /usr - Contains user applications and their dependencies
    Example:
        Python, Git, gcc, vim
    Think of it as the location for most installed softwares

    /dev - represents hardware devices as files.
    Example:
        /dev/sda, /dev/null, /dev/random
    In linux, many hardware devics are accessed through files.

    /proc - virtual filesystem created by linux
    It provides information about:
        CPU, Memory, running processes, kernal state
    It doesnot store regular files on disk - its generated dynamically

    /sys - another virtual filesystem exposing kernal and hardware information, often used for device configuration and inspection

### Absolute vs relative path
    Absolute path: 
        /home/koushik/projects/app.py
        Starts from /.
    Relative path:
        projects/app.py
        Starts from your current working directory.
    To see current working directory:
        use pwd

### Questions
    1.What is linux kernal?
        The Linux kernel is the core of the operating system. It acts as an intermediary between applications and the hardware by managing resources such as CPU, memory, disks, files, processes, and networking through system calls.

    2.Why doesn't Python access the disk directly?
        Python cannot access hardware directly because user-space applications are isolated from hardware for security and stability. When Python needs to read a file, allocate memory, or open a network socket, it makes system calls to the Linux kernel, which performs the operation and returns the result.
    
    3.Difference between / and /root
        / Root directory and /root home directory for root user

    4.What is /etc?
        contains system configuration files like /etc/hosts, /etc/passwd, /etc/fstab, and SSH configuration files.
    
    5.Why is /proc called a virtual filesystem?
        /proc is called a virtual filesystem because its files are not actually stored on disk. The Linux kernel generates them dynamically in memory to expose information about running processes, CPU, memory, and kernel state.
    
    6.Absoulte vs relative path?
        An absolute path always starts from the root directory (/) and uniquely identifies a file regardless of the current working directory. A relative path is interpreted from the current working directory.
    
### Listing commands
    ls -> lists files and folders

    ls -l -> shows detailed information
        -rw-r--r-- 1 koushik users 520 Jul 10 10:00 requirements.txt
        drwxr-xr-x 2 koushik users 4096 Jul 10 11:00 app

    ls -a -> Shows hidden files
        Files begninning with . are hidden
    
    ls -lh -> Human readable file sizes Instead of 2543232 you will see 2.4M

    ls -R -> Recursive Listing, Shows everything inside subdirectories., Useful for quickly viewing a project structure.

### Commands
    cd -> change directory
    cd .. -> Go back 
    cd ~ -> Go home directory
    cd / -> Go root directory

    mkdir -> make directory
        mkdir logs
        logs/
    
    mkdir -p -> Creates nested directories
        mkdir -p project/backend/api
    
    touch - creates an empty file
        touch app.py creates a app.py empty file
    Create multiple files
        touch app.py create.py hello.py
    
    cp - copy a file
        cp app.py backup.py
        copy a directory
            cp -r test/ test_backup/
        -r = recursive
    
    mv - Move files
        mv app.py backup/
        
    Rename a file
        mv old.py new.py
    
    rm - delete a file
        rm app.py
        Delete a directory
        rm -r test
        Delete without confirmation
        rm -rf test
    
    tree - Displays directory structure visually

    Wildcards
        Match every python file
            ls *.py -> a.py, b.py, test.py 
        Delete all log files
            rm *.py
        Match exactly one character
            ?.py -> a.py, b.py, test.py(invalid)
        Match a set of character
            ls files[1-5].py -> files1.py, ...,files5.py
    Find
        searches for files and directories
        find . -name "*.py" -> app/views.py, app/manage.py
        find . -name "*.log"
    Find directories
        find . -type d
    Find files larger than 100mb
        find . -size +100M
    
    locate -> A very fast file search command
        locate manage.py
    Unlike find it searches prebuilt database of file locations, making it much faster.
    If a recently created file isn't found, you may need to update the database (typically with updatedb, which requires appropriate permissions).

    Realworld backend flow
        pwd                 # Where am I?
        ls -la              # View all files, including hidden ones
        cd my_project       # Enter the project directory
        find . -name "*.log" # Locate log files
        cp .env .env.backup # Backup environment file
        mkdir backups       # Create a backup directory
        mv error.log backups/ # Archive an old log
        tree                # Inspect the project structure

#### Questions
    1. Difference between cp and mv
        cp creates a duplicate of a file or directory while keeping the original intact. mv moves a file or directory to a new location or renames it. After mv, the original location no longer contains the file.
    2.Why cp -r?
        cp alone copies files.
        Directories require:
        cp -r project project_backup
        because -r means recursive (copy everything inside the directory).
    3.mkdir -p
        Used to create nested directories
        It doesn't fail if the parent directories don't exist.
    4.Difference between find and locate
        find
            searches the actul file system
            slower
            always up to date
        locate
            searches a prebuilt database of file locations
            very fast
            Database may be outdated until refreshed
    5.Difference between ls, ls -a, ls -l
        ls -> lists files and folders
        ls -l -> shows permissions, owner, group, file size, last modified, file name
    6.?.py
        It matches exactly one character.
    7.*
        matches zero or more characters

### Permissions
    when i use ls -l
    -rwxr-xr-- 1 koushik developers 2048 Jul 10 10:15 app.py
    - -> Tells us it a file type
    - -> file type, d -> Directory, l -> Symbolic link, c -> Character device, b -> Block device
    Ignore first character -
    now divide this into three groups
        rwx -> owner
        r-x -> group
        r-- -> others
    Suppose -rwxr-xr-- 1 koushik developers app.py
        koushik is owner
        developer are group
        Everyone else on the system falls under Others.

    Permission Meaning
        Read(r)
            value=4
            Allows:
                Read a file
                view directory contents
        Write(w)
            value=2
            Allows:
                Modify a file
                Write a file in directory
                Delete a file(subject to directory permissions)
        Execute(x)
            value=1
            Allows:
                Execute a program or script
                Enter (traverse) a directory
    
    Understaning rwx
        rwx -> read,write,execute
        rw- -> read,write
        r-x -> read,execute
        r-- -> read
        --- -> No permission
    
    Numeric Permissions
        rwx -> 4+2+1 = 7
        rw- -> 4+2 = 6
        r-x -> 4+1 = 5
        r-- -> 4
    
    Common permission values
        755 -> rwx r-x r-x
        owner: Everything, groups and others: read + execute
        644 -> rw- r-- r--
        owner: read+write, groups and others: only read
        700 -> rwx --- ---
        owner: everthing
        600 -> rw- --- ---
        Used for secerts like SSH private keys, Cerifications, Passwords
    
    Change permissions
        suppose -rw-r--r--
        chmod 755 app.py -> -rwx-r-x-r-x
        Want this: rw-r--r--
        run chmod 644 app.py
    
    Symbolic chmod
        Instead of numbers
            chmod +x app.py -> from -rw-r--r-- to -rwx-r-x-r-x
        Adds execution permission to every user
        Remove execution permissions
            chmod -x app.py
        Add write permissions
            chmod +w app.py
    
    Change ownership
        sudo chown ubuntu app.py
    Owner becomes ubuntu user

    Change ownership and group
        sudo chown ubuntu:hacker app.py
        ubuntu - owner
        hacker - group
    
    sudo -> sudo runs the command with administrator (root) privilege

#### Why is 777 dangerous?
    rwxrwxrwx
    Everyone can: Read,write,execute
    image your django project has settings.py file that has 777 permissions
    Any user on the system can modify it. That's a significant security risk.

    It grants full permissions to everyone, increasing the risk of unauthorized modification or execution. It's generally better to follow the principle of least privilege and grant only the permissions that are actually needed.
    
#### Questions
    1.What does -rwxr-xr-- mean?
        - regular file
        owner has everything(read,write,execute)
        Group can read,exeute
        Other has only read permissions
    2.Difference between 755 and 644?
        644 -> owner has read,write. group and other have read permissions
        Used for normal files
        755 -> owner has everything. group and others have read, execute permissions
        Used for directories and executable files
    3.Why is 777 dangerous?
        Gives all the permission to every user,increasing the risk of unauthorized modification or execution
        Always use least privilege for security.
    4.Difference between chmod and chown
        Chmod -> changes permissions
        Chown -> change ownership and or/group of the file
    5.What is sudo?
        sudo temporarily executes a command with superuser (root) privileges without logging in as the root user.
    6.Owner, Group, Others?
        Owner: simply the user who owns the file, which can be any user.
            suppose i have created temp.txt i will be the owner
        Group: can be a group of people like developers group, devops group
        others: Everyone else.
        Root is simply a special administrator account. not always a owner
    7.Why does a directory need Execute permission?
        The execute (x) permission on a directory allows a user to traverse or enter the directory. Without execute permission, even if the directory has read permission, the user cannot access files inside it or use cd to enter it.
    For directories:
        r -> List names inside the directory.
        x -> Enter/traverse the directory.
        w -> Create, rename, or delete files in the directory (subject to ownership and other rules).

## Process in linux
    A process is simply a program that is currently running.
        program(stored on disk) -> executed -> program(running in memory)
    Process lifecyle:
        program -> process created -> Running -> waiting for i/o -> running again -> finished
    When a django application receives request
        running -> waiting for postgresql -> running -> return response -> waiting
    The process keeps on switching between running and waiting.
    Every process has an id that is called PID(process id).
    You use the PID to inspect or stop a process.

    View running process
        ps
        ps -ef(This the command we use most)
            -e = show every process on system
            -f = show full format output(UID, PID, PPID, start time, command, etc)
        This is why it's commonly used with grep.
    Instead of looking for every process we ca grep the processes
        ps -ef | grep python
        That gives python process
    
    Top
        Displays live system information. CPU Usage, Memory usage, Running processes, Load average
    htop
        improved version of top
        Advantages:
            Easier to read
            Colored interface
            Search process
            kill processes interactively
        most linux admins prefer this htop
### Foreground vs background Process
    Run: python manage.py runserver
    Terminal is occupied
    This is foreground process
    You cant use the terminal until you stop it

    Run: python manage.py runserver &
    Server keeps running
    Terminal becomes available
    This is a background process

## Networking
    Networking flow:
        Browser -> DNS(myapp.com -> IP address) -> Internet -> AWS EC2 -> Ngnix -> Gunicorn -> Django -> Postgres
    If something is breaking we use networking commands to find where it is

    1.Ping
        Check another machine is reachable
            ping google.com
        We get this
            64 bytes from ...
            time=22 ms
        This means
            DNS resolved google.com
            Network connectivity exists
            The remote host responded
        When your django application cant make a external api call
            ping api.github.com
            If this fails:
                No internet
                DNS problem
                Firewall
                The host blocks ICMP(ping)
            Important: Many production servers disable ping, so a failed ping doesn't always mean the service is down.
    2.curl
        curl is a command-line tool used to send HTTP/HTTPS requests to web servers and APIs. It is commonly used to test REST APIs, download data, upload files, and debug network connectivity

        curl http://www.google.com/ it does get request
        curl -I http://www.google.com/ we HTTP/1.1 200 OK headers as well
        
        By default curl sends HTTP request but we can send other methods too
        curl -X POST -H "Content-Type: application/json" -d '{"name":"laptop"}' http://www.google.com/ 

        Why do we use this?
            Suppose the django application calls an external api
                response = requests.get("https://api.github.com")
            if it fails i can run
                curl https://api.github.com
            if curl succeeds, the network is likely fine, and the issue is probably in your application code or configuration.
            If curl also fails, the issue is likely with network connectivity, DNS, firewall rules, proxy settings, or the remote service itself.
    
    3.wget
        wget is another command-line tool used to download files from the internet using HTTP, HTTPS, and FTP protocols.
        Unlike curl, which is mainly used for sending and testing HTTP requests, wget is primarily designed for downloading content.
        Example:
            wget https://example.com/file.zip
        Download and save with a different name:
            wget -O myfile.zip https://example.com/file.zip
        Resume an interrupted download:
            wget -c https://example.com/file.zip
        Download in background
            wget -b https://example.com/file.zip
    
        Curl vs wget
        Curl:
            Curl primary purpose Send http/https request to API's
            Supports HTTP/HTTPS requests
            Support FTP
            Default Action Prints response to terminal
            Best for Rest APIs
            Resume downloads Limited
        Wget:
            Primary purpose Download files
            Supports both http and https requests
            Suppots FTP
            Default action: Saves response to file
            Best for Rest APIs: Possible, but less convenient
            Resume download Excellent support
        
            wget is a command-line utility used to download files from web servers over HTTP, HTTPS, or FTP. It supports features like recursive downloading, resuming interrupted downloads, and background downloads, making it useful for downloading large files or entire websites.
    
    4. SS
        SS stands for Socket Statistics.
        It is a Linux command used to display network sockets and connections. It is the modern replacement for netstat and is faster.
        we can use it to check
            Which ports are listening
            Wheather the application is running or not
            Active TCP/UDP connections
            Which process is using a specific port
        Show all tcp connections
            ss -t
        Show all udp connections
            ss -u
        show listening ports
            ss -l
        show listening tcp ports
            ss -lt
        show listening ports with process name
            ss -ltnp
        -n Show numeric addresses and port numbers (don't resolve names).
        -p Show the process (program) using the socket, including the PID and process name.
        To check if django application listening to 8000 port
            ss -ltnp | grep 8000
            LISTEN 0      4096         0.0.0.0:8002       0.0.0.0:*

            ss -ltun | grep 3000
            -ltun means listening socket tcp socket/udp socket show numeric address and posts numbers
        
        ss -ltunp displays all listening TCP and UDP sockets with numeric IP addresses and port numbers, along with the process name and PID that own each socket. It's commonly used to verify which services are listening on which ports and which processes are using them.
    
    5. Dig
        dig stands for Domain Information Groper.
        It is a Linux command used to query DNS (Domain Name System) servers. It helps you find information such as the IP address of a domain, mail servers, name servers, and other DNS records.
        As a backend developer, dig is commonly used to troubleshoot DNS resolution issues.

        dig google.com
        ;; QUESTION SECTION:; google.com.
        ;; ANSWER SECTION:
        google.com.             93      IN      A       172.253.118.138

        Dig Vs Ping Vs Curl
            dig google.com -> Resolves the domain name to an IP address (DNS lookup).
            ping google.com -> Checks whether the host is reachable using ICMP packets.
            curl google.com -> Sends an actual HTTP/HTTPS request to the server.
        In debugging sense:
            DNS: Can I resolve the hostname? -> dig
            Network: Can I reach the host? -> ping (if ICMP isn't blocked)
            Application: Can I make an HTTP/HTTPS request? -> curl
    
    6. Nslookup
        nslookup is another DNS lookup tool. Like dig, it is used to check whether a domain name can be resolved to an IP address.
        The main reason to use nslookup is DNS troubleshooting.
        nslookup google.com
            Server:         127.0.0.53
            Address:        127.0.0.53#53

            Non-authoritative answer:
            Name:   google.com
            Address: 64.233.170.101
            Name:   google.com
            Address: 64.233.170.102
        
        nslookup vs dig
        nslookup
            simple output
            Easy for quick dns checks
            Available on windows, linux and macos
            Basic debugging
        dig
            More detailed output
            Better for indepth DNS troubleshooting
            Common for linux and macos
            Peffered by linux administators for advanced dns analysis
    
    7. traceroute
        Shows the path packets take to reach a destination.
        traceroute google.com
        Useful when diagnosing network routing issues or unexpected latency.
    
### Production system
    my app is running at https://mycompany.com isn't working
    
    Step1:
        Check is the ngnix port listening
            ss -ltun
        No port 80? Nginix isn't running
        sudo systemctl restart ngnix
    Step2:
        Still failing
        check application is working or not
        curl localhost
        If we get 200 ok the issue may be AWS Security Group, Load Balancer, DNS, Firewall
    Step3:
        check dns
        nslookup mycompany.com or dig mycompany.com
    Step4:
        Check the application
        curl localhost:8000
        if it fails probably the django application has issue
    
## Linux logs and text processing
    1.cat -> Displays entire content of the file
        cat app.log
    when to use:
        small files
        Configuration files
        Quick inspection
    
    2.less -> view a file page by page
        less app.log
    Useful keys:
        Space -> next page
        b -> previous page
        /error -> search an error
        n -> next search result
        q -> Quit
    
    3.head -> shows first few lines(default is 10 lines)
        head app.log
    To view first 20 lines
        head -20 app.log
    
    4.tail -> shows last few lines(10 default)
        tail app.log
    To view last 20 lines
        tail -20 app.log or tail -n 20 app.log
    
    5.tail -f -> The terminal stays open and shows new log entries as they have written
        tail -f app.log
    To view last 100 logs and live data
        tail -100 -f app.log or tail -n 100 -f app.log
    
    6.grep -> Searches for text
    Suppose your logs have 10k lines
    Find only errors.
        grep ERROR app.log
    Ignore case:
        grep -i error app.log
    Count matches:
        grep -c ERROR app.log
        output: 15
    
    7.Combine commands using pipelines(|)
        ps -ef | grep python
    The | (pipe) sends output of the first command as input to the second.
        ss -ltunp | grep 3000
    
    8.journalctl
        Many services (like Docker, Ngnix, Gunicorn) log through systemd
        View logs:
            journalctl
        View logs for Ngnix:
            journalctl -u ngnix
        View logs for Gunicorn:
            journalctl -u gunicorn
        Show only last 100 lines:
            journalctl -u gunicorn -n 100
        Live logs:
            journalctl -u gunicorn -f
    
    9.awk -> extracts column from a text
        ps -ef | awk '{print $2}'
    This prints the second column which is PID

    10.sed -> Edits or replaces the text
        Replace Error with Warn
        sed 's/Error/Warn' app.log
    Replace every occurrence:
        sed 's/Error/Warn/g' app.log
    The g means "global" (all occurrences on each line).

### Production debugging senario
    The API returns 500 internal error
    1.Check process is runnig or not
        ps -ef | grep gunicorn
    2.Check wheather it is listening
        ss -ltunp | grep 8000
    3.Watch logs
        journal -u gunicorn -f
    4.call an api while watching logs
        curl http://localhost:8000/
    you might see: Database connection failed
    Now you know the problem isn't Django - it's the database connection.

### Questions
    1.Why do we use tail -f instead of cat?
        tail -f continuously monitors new log entries as they are written, making it ideal for debugging live applications. cat prints the entire file once and then exits, which is impractical for large or continuously growing log files.
    2.grep ERROR app.log
        It searches for all lines containing: ERROR
    3.Pipe (|)
        The output of the first command is passed as an input to second command
        cat app.log | grep ERROR
    4.journalctl
        journalctl is used to view logs managed by systemd, including services like Docker, Nginx, Gunicorn, PostgreSQL, and many others.
    5.head vs tail
        head displays first few lines and tail displays last few lines
    6.HTTP 500 Debugging
        Check process -> check listening -> check live logs -> call api -> observe logs

## Shell scripting
    The kernal talks to hardware
    The shell is a program that lets you communicate with the kernal by typing commands.
    
    ls -> Shell interprets the command and asks kernal to execute it.

    What is Bash?
        Bourne Again SHell
    It is the most commonly used shell on Linux.

    Instead of typing 20 commands line by line i can put this commands in file as deploy.sh
        #!/bin/bash
        echo "Starting Application"
        python manage.py migrate
        python manage.py collectstatic
        systemctl restart gunicorn
        echo "Deployment Completed"
    
    bash deploy.sh or chmod +x deploy.sh
                       ./deploy.sh
    This is why deployment automation works

### Shell scripting basics
    The first line is called Shebang #!/bin/bash
    Tells linux to execute the script using bash
    with out it ./deploy.sh doesnot know which interpretor to use

    Comments: # ignoned by bash, useful for documentation

    1.Variables:
        NAME="Koushik"
        NAME = "Koushik" # spaces are not allowed
    To print name:
        echo $NAME
    
    2.User Input
        read NAME
        echo $NAME
    Save this code in app.log file and now execute bash app.log
    koushik
    output: koushik

    2.1.User friendly input
        read -p "Enter Name: " NAME
        echo "Hello $NAME"
    Input:
        Enter Name: koushik
    Output:
        hello koushik
    
    3.Echo
    print text
        echo "hello"
    print variables
        echo $NAME
    combine
        echo "hello $NAME"
    
    4.Command Substitution
    Store output of another command
        CURRENT=$(pwd)
        echo $CURRENT
    
### 5.If statement
        if [ 5 -gt 3 ]
        then
            echo "True"
        fi
    fi is the reverse of if, it marks the end of if block
    Compare Numbers:
        -eq Equal
        -ne Not Equal
        -gt Greater than
        -lt less than
        -ge greater than or equal
        -le less than or equal
    
    Codes:
    code1:
        A=5
        if [ $A -gt 3 ]
        then
            echo "True"
        fi
    code2:(string comparison)
        NAME="Koushik"
        if [ $NAME = "Koushik" ]
        then
            echo "Matched"
        fi
    if-else:
        if [ 5 -ge 3 ]
        then
            echo "True"
        else
            echo "False"
        fi
    if-elif-else
        MARKS=95
        if [ $MARKS -ge 90 ]
        then
            echo "First class"
        elif [ $MARKS -ge 80 ]
        then
            echo "Second class"
        else
            echo "Third class"
        fi
    In moden bash we can use arithmetic operators directly
    code:
        MARKS=95
        if (( MARKS > 90 )); then
            echo "First class"
        elif (( MARKS > 80 )); then
            echo "Second class"
        else
            echo "Third class"
        fi
### For loop
    codes:
        for i in 1 2 3 4 5
        do
            echo $i
        done

    Another way
        for i in {1..5}
        do
            echo $1
        done

    Step increment
        for i in {0..10..2}
        do
            echo "$i" # if we use quotes even if the value is error it wont throw
        done    
        output: 0,2,4,6,..10

    C type style(similar to python)(recommended)
        for((i=1;i<10;i++))
        do
            echo $i
        done

    print all the .py files
        for i in *.py
        do
            echo $i
        done

### While loop
    COUNT=1
    while [ $COUNT -ge 5 ]
    do
        echo $COUNT
        COUNT=$((COUNT+1))
    done
    
    Arithemetic syntax(recommended)
    COUNT=1
    while (( COUNT <= 5 ))
    do
        echo $COUNT
        ((COUNT++))
    done

### Function
    Create simple greet function
        greet() {
            echo "Hello"
        }
        greet #calling the function
    Creat greet function with 2 name inputs
        greet() {
            echo "Hello $1"
            echo "hi $2"
        }
        great koushik abc
        output:
            Hello koushik
            hi abc
    Check number
        check_number() {
            if [ $1 -ge $2 ]
            then
                return 0
            else
                return 1
            fi
        }
        check_number 2 3
        echo $?
    $? contains the exit status of the last command:
    0 -> Success
    Non-zero -> Failure

    Sum function using local variable
    sum() {
        local RESULT=$(($1+$2))
        echo "Sum=$RESULT"
    }
    sum 10 20
    output: Sum=30

### $?
    Linux commands that contains exit code of last command
    0 -> success
    Non-zero -> Failure

    mkdir project
    $?
    Output: 0 -> sucess, 1 -> directory already exists

    Mainly used for ci/cd pipelines
    code:
        python manage.py test
        if [ $? eq 0 ]
        then
            echo "Deploy sucess"
        else
            echo "Failed test cases"
        fi

    Check if file exists are not
        if [ -f app.log ]
    Check if directory exists are not
        if [ -d dir ]

### Production script
    #!/bin/bash
    if ps -ef | grep gunicorn | grep -v grep > /dev/null
    then
        echo "Gunicorn Running"
    else
        echo "Restarting Gunicorn"
        sudo systemctl restart gunicorn
    fi

### concepts
    #!/bin/bash -> Shebang
    echo -> print text
    read -> user input
    $VAR -> Access varibale
    $(command) -> Store command output
    if -> conditional logic
    for -> Loop over values
    while -> Repeat while condition is true
    $? -> Exit status code of previous executed command
    -f -> Check if file exists
    -d -> check if directoy exists

### Questions
    1.why do we use grep -v grep command in this ps -ef | grep gunicorn | grep -v grep?
    ps -ef | grep gunicorn
    It will give grep process as well along with gunicron
    Example:
    ps -ef | grep python
        root       212     1  0 Jul15 ?        00:00:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-for-signal
        koushik   3478  3458  0 04:13 pts/5    00:00:00 grep --color=auto python
    Notice we got 2 processes
    To invert grep command we can use
    ps -ef | grep python | grep -v grep
        root       212     1  0 Jul15 ?        00:00:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-for-signal
    we got only one process

    2.Imagine Gunicorn crashes
        Start script -> check if the gunicron process exists -> yes -> print "Gunicorn is running" -> Exit
        No -> Restart Gunicron -> Check again -> if running -> print "Gunicorn restarted successfully" -> else -> print "Gunicorn failed to restart"
    
    3.why do we use quotes for the variables?
        NAME=""
        if [ $NAME = "Koushik" ]
        then
            echo "Matched"
        else
            echo "Mismactched"
        fi
        I will get error since NAME Is empty the if statment will work if [ = "Koushik" ] fail
        if i use quote it will treat if [ "" = "Koushik" ] will work

        Even to avoid name spliting
        $NAME="koushik g"
        if [ "$NAME" = "koushik g" ]
    
    4.difference between -f, -d, -e?
        -f -> checks if file exists are not
        -d -> checks if directory exists
        -e -> checks the path if exists are not

        Example: app.py file, logs directory
        -f app.py # checks app.py exists or not
        -f logs #fail since logs is directory

        -d logs #checks logs directory exists or not
        -d app.py # fails since it is file

        -e logs #logs path exists true
        -e app.py #app.py path exists true

    5.write a script to see if logs exists or not, if not create a logs directory?
        if [ -d "logs" ]
        then
            echo "Logs directory already exists"
        else
            if mkdir logs
            then
                echo "Logs directory created successfully"
            else
                echo "Failed to create logs directory"
            fi
        fi
    
    6.write a script for this
        Check if Gunicorn is running.
        If it is running:
        Print "Gunicorn is already running".
        If it is not running:
        Restart Gunicorn using systemctl.
        After restarting:
        Check again whether it's running.
        Print either:
        "Gunicorn restarted successfully"
        or "Gunicorn failed to start".
    Code:
        #!/bin/bash

        GUNICORN_COMMAND=$(ps -ef | grep gunicorn | grep -v grep)

        if [ -n "$GUNICORN_COMMAND" ]
        then
            echo "Gunicorn is already running"
        else
            echo "Restarting Gunicorn..."

            if sudo systemctl restart gunicorn
            then
                GUNICORN_COMMAND=$(ps -ef | grep gunicorn | grep -v grep)

                if [ -n "$GUNICORN_COMMAND" ]
                then
                    echo "Gunicorn restarted successfully"
                else
                    echo "Gunicorn failed to start"
                fi
            else
                echo "Restart command failed"
            fi
        fi

    7.In the previous script used mkdir logs for if statement but dont we use if sudo sytemctl restart gunicorn to check if gunicorn restarted or not why again check process again?
        
        mkdir logs gives 0 success, non-zero if permission issue
        sudo systemctl restart gunicorn the gunicorn will give 0 sucess but soon if there is any code error it will fail so it is always better to check process gunicorn is running or not

## Sytemd and Systemctl (Service Management)
    why do we need services?
    Imagine i have ran the application using
        python manage.py runserver
    The application stops when i close terminal, ssh out of server, ec2 rebooted
    That's why we dont run applications like that in production
    Instead linux runs them as services.
    Example:
        Nginx, postgresql, redis, docker, gunicorn, ssh
    These are managed by systemd

    what is a Systemd?
        systemd is the service manager in the modern linux.
        Its also the PID 1
    Its responsibility includes:
        Starting services during boot
        Stopping services
        Restarting services
        Monitoring services
        Restarting crashed services
        Managing logs(through journalctl)
    
    What is a systemctl?
        it is a commandline tool used to communicate with systemd.
        you dont need to interact with systemd directly.
        use:
        systemctl
    
### Systemctl commands
    1.Check status of ngnix
        systemctl status ngnix
        output:
            nginx.service - A high performance web server
            Active: active (running)
            Main PID: 1050
        Tells you:
            Is the service running?
            pid
            recent logs, startup time
    
    2.Start a service
        sudo systemctl start ngnix
    
    3.Stop a service
        sudo systemctl stop ngnix
    
    4.Restart a service
        sudo systemctl restart ngnix
    
    5.Reload vs restart
        restart
        sudo systemctl restart ngnix
            stops the service
            starts it again
            connections may be interputed
        reload
        sudo systemctl reload ngnix
            Reloads the config without fully stopping the service
            useful when changing:
                ngnix.conf
                virtual hosts
    
    6.Enable a service
        Suppose Ec2 reboots will ngnix start automatically?
        No we need to enable the service
            sudo systemctl enable ngnix
        When ec2 reboots the ngnix will start automaticlly

    7.Disable a service
        sudo systemctl disable ngnix
    
    8.Check wheather a service is enabled
        sudo systemctl is-enabled ngnix
    
    9.Check wheather a service is running
        systemctl is-active ngnix
            give active or inactive
            much better than ps
    
        Script friendly
            if systemctl is-active --quiet nginx
            then
                echo "Running"
            fi
    
    10.view logs
        journalctl -u ngnix

### Service unit files
    Every service has a configuration file.
    usally located at: /etc/systemd/system/ or /lib/systemd/system/
    Example:
        gunicorn.service
        Content:
            [Unit]
            Description=Gunicorn

            [Service]
            User=ubuntu
            WorkingDirectory=/home/ubuntu/project
            ExecStart=/home/ubuntu/venv/bin/gunicorn config.wsgi

            [Install]
            WantedBy=multi-user.target

        [Unit]: contains general information
        [Service]: realwork is here
            user: ubuntu
            working dir it is just like cd /home/ubuntu/project
            execution command: similar to running gunicorn config.wsgi
    
### Suppose you modify service configuration files
    I have changed contents in gunicorn configuration file
    Linux automatically wont detect the changes
        sudo systemctl daemon-reload
    This tells systemd to reload all its service definations
    Then restart gunicron
        sudo systemctl restart gunicorn

### Real production issue
    The deployment is done, the website is down
    Check gunicorn active or not -> Check gunicorn logs -> fix configuration or dependency issues -> reload system configuration files if edited gunicorn config files -> restart service -> check is the service active

    1.Check the service status
        sudo systemctl status gunicorn
    2.If its inactive check logs
        journalctl -u gunicorn -f  
    3.fix the issue (configuration or missing dependency)
    4.reload service definations if you edited the service file
        sudo systemctl daemon-reload
    5.Restart the service
        sudo systemctl restart gunicorn
    6.Check is active
        sudo systemctl is-active gunicorn

### Questions
    1.What is systemd?
        systemd is the init system and service manager in modern Linux. It starts services during boot, manages their lifecycle (start, stop, restart), monitors them, and manages their logs through journalctl. It also runs as PID 1.
    2.Why use systemctl instead of python manage.py runserver?
        if we close the teminal or ssh out of server or linux rebooted the application will not work anymore if we run through systemctl as a service then all the problems will be fixed
            python manage.py runserver is:
        Development server
        Single process
        Not designed for production
        production uses: Internet -> ngnix -> gunicorn -> django
        Production uses gunicorn itself is managed by systemctl
    3.restart vs reload?
        restart stops the application and start the application, may interrupt the service, reload not going to stop the application, reloads the config no down time of service
    4.Purpose of enable
        systemctl enable configures the service to start automatically during system boot.
    5.daemon-reload
        if we have changed the service configuration the systemd doesnot know we did some changes, daemon-reload lets systemd to reload all service definations
    6.Why systemctl is-active instead of ps -ef | grep?
        ps only tells us:
        "A process exists."
        It doesn't tell us whether:
            The service is healthy
            It failed during startup
            It is managed by systemd
        systemctl is-active tells us the actual service state.
        it can give status like active, inactive, failed, activating, deactivating
        So it's more accurate for services managed by systemd.

## Debug production
    My application deployed like this
    Internet -> AWS load balancer(optional) -> Ngnix -> Gunicorn -> Django -> Postgresql
    User sees:
        502 bad gateway
        Nginx cannot get a valid response from Gunicorn (its upstream).
    
    Step1: Check Ngnix
        systemctl status ngnix
        look for errors
        journalctl -u ngnix -100

    Step2: Check gunicorn
        systemctl status gunicorn
            if inactive: check logs
            journalctl -u ngnix -100
            Typical errors can be:
                ModuleNotFoundError
                Permission denied
                Address already in use
                ImportError
    Step3: check gunicorn is listening
        ss -ltun | grep 8000
        even if gunicorn is listening, is it actually running on expected port?
        if not ngnix cannot connect to it.
    Step4: Test the backend directly
        curl http://localhost:8000/
        possible results:
            200 ok
        connection refused:
            Gunicorn is not serving requests
    Step5: check resources
        htop
            cpu, memory, swap
            If memory is exhausted, Gunicorn may have been killed by the OOM killer.
    Step6: Check database
        if gunicorn logs this
            OperationalError
            could not connect to PostgreSQL
        then:
            systemctl status postgresql
        is postgres listening?
            ps -ef | grep postgres
    Step7: Only after confirming the applicaiton is running locally would check this:
        dig mycompany.com
        DNS is usually not the cause of a 502 Bad Gateway.
    Step8: Network / Firewall
        curl localhost:8000 works but users cant access the application
        then investigate:
            AWS Security Groups
            Network ACLs
            Load Balancer
            Firewall
    Step9: If Nginx CPU usage is 100%
        Investigate why CPU usage is high before restarting.
        High CPU could be caused by:
        Traffic spike
        Infinite redirect loop
        Misconfiguration
        Upstream timeout
        Attack (e.g., DDoS)
    Restarting may temporarily hide the symptom without fixing the cause

## Disk and memory monitoring
    1.df(Disk Filesystem)
        df -h
        it give filesystem, size, used, avail, Use%, mounted on
    why -h?
        without it 20971520
        with -h 20G human readable
    2.du(Disk Usage)
        Shows how much space files and directories consume.
        du -sh logs #2.4 gb logs

        Check all folders
        du -sh *
        output:
            3G logs
            2G uploads
            50M app
    3.df vs du
        df: shows disk usage of entire filesystem
        du: shows disk usage of specific directory or file
    4.free
        free -h
        shows rams usage
        total 8gb, used 5gb, free 2gb, swap 1 gb
    5.Swap
        Swap is disk space used as extra memory when RAM is full.
        Ram Full -> linux moves less used memory pages to disk -> swap
        swap is much slower than ram
        High swap usage often means the server needs more memory or processes are using too much RAM.
    6.vmstat
        Shows virtual memory and CPU statistics.
        vmstat
        It gives information about:
            Running processes
            Blocked processes
            Memory
            Swap
            CPU
    7.iostat
        Shows disk I/O performance.
        iostat
        Useful when the application is waiting on slow disk operations.
        Example:
        PostgreSQL is slow because disk writes are saturated.
        Large backups are consuming disk bandwidth.

### Scenarios
    Scenario 1:
        users cannot upload files
        check disk
            df -h
        if the usage is 100%, the disk is full
        Next:
            du -sh *
            it shows 18G logs
        Solution:
            Rotate or clean old logs.
            Compress/archive logs.
            Investigate why logs are growing so quickly.
    Scenario 2:
        Gunicorn keeps getting killed.
        check memory:
            free -h
        The disk has 25M
        The server has almost no free memory.
        Possible solutions:
            Reduce Gunicorn workers
            Increase Instance size
            Investigate memory tasks
    Scenario 3:
        vmstat
        The application feels slow.
        The cpu is only 10% use
        Memory usage is fine.
        check: iostat
        Disk utilization is very high
        The bottleneck is the storage subsystem, not the CPU.

### Questions
    1.Difference between df -h and du -sh logs
        df -h shows disk usage of the entire filesystem (total, used, available, and usage percentage).
        du -sh logs shows how much disk space the specific logs directory is using.
    2.Disk is 100%
        File uploads fail
        Logs cannot be written
        PostgreSQL may fail to write data or WAL files
        Temporary files cannot be created
        Backups may fail
        Some applications may even crash
    3.Free RAM is 20 MB
        ram exhausted, investigate memory leaks or reduce Gunicorn workers
        can add:
            Check which process is consuming memory using top or htop
            Scale up the instance if memory is consistently insufficient
    4.What is swap?
        When RAM is full, Linux moves less frequently used memory pages to disk (swap). Since disk access is much slower than RAM, heavy swap usage can significantly reduce application performance.
    5.iostat
        Use iostat when you suspect disk I/O is the bottleneck. Unlike top, which focuses on CPU and memory, iostat helps identify whether slow reads or writes are causing application performance issues.

## Cron jobs(Task Scheduling)
    Imagine you need to:
        Take a postgresql backup every day at 2am
        delete logs every sunday
        Run a cleanup script every hour
        send reports every morning
    You dont want to ssh manually into the server

    what is a cron?
        Cron is job-scheduler in linux.
        it automatically runs scripts or commands at scheduled times.
    
    what is crontab?
        crontab (Cron Table) is the file where you define scheduled jobs.
    
    Commands:
        view all your cron jobs -> crontab -l
        edit your cron job -> crontab -e
        remove all cron jobs ->  crontab -r
    
    Every cron job has 5 time fields followed by command
        Minute(0-59) Hour(0-23) Day_of_month(1-31) Month(1-12) Day_of_week(0-7)(sunday=0 or 7)
    
    To Run every minute
        * * * * * /home/ubuntu/script.sh
    
    Every day at 2 am
        * 2 * * * /home/ubuntu/backup.sh
        Meaning: Minute: 0, Hour: 2, Every day, every month, every weekday
    
    Every sunday at 3 am
        * 3 * * 0 /home/ubuntu/cleanup.sh
    
    Every 5 minutes
        */5 * * * * /home/ubuntu/script.sh
        why */5 defines for every 5 minutes like 9:00,9:05,9:10,9:15...
        if i use direct 5
        5 * * * * /home/ubuntu/script.sh
        The script will run every hour at 5 minute like 9:05,10:05,11:05
    
    Every hour
        0 * * * * /home/ubuntu/script.sh mean every 0th minute like 10:00, 11:00
    
    Common ones:
        Every 2 hours            * */2 * * *
        Every day at midnight    0 0 * * *
        every day at 2:30am      30 2 * * *
        Every sunday at 3 am     0 3 * * 0
    
    Redirect output to log file
    0 2 * * * /home/ubuntu/script.sh >> /var/log/backup.log 2>&1
    
    2>&1 redirect standard error(2) to the same place of standard output(1)
    Now the logs contains both success and failure logs

    Run python with cron
    0 2 * * * /usr/bin/python3 /home/ubuntu/script.py

### Production scenarios
    1. Daily Database Backup
        0 2 * * * /home/ubuntu/scripts/backup.sh
    2.Delete Old Logs
        0 0 * * 0 find /var/log/myapp -name "*.log" -mtime +30 -delete
        Every sunday at midnight, delete log files older than 30 days
    3.Health Check
        */10 * * * * /home/ubuntu/scripts/check_gunicorn.sh
        The script checks whether Gunicorn is running and restarts it if necessary.
    4.Cleanup Temporary Files
        0 1 * * * rm -rf /tmp/myapp/*
        Runs daily at 1 AM.

    Common Mistakes:
        1.Always use absolute path instead of relative path for cron jobs
            * * * * * script.sh use this * * * * * /home/ubuntu/script.sh
        2.Check the file is executable or not
            if not make it executable
            chmod +x /home/ubuntu/script.sh

### Questions
    1. crontab -l vs crontab -e
        crontab -l -> Lists the current user's cron jobs.
        crontab -e -> Opens the current user's crontab for editing.
    2.Why absolute paths?
        Cron runs with a minimal environment and a different working directory, so relative paths may not resolve correctly. Using absolute paths ensures the correct script and executables are found.
    3.Script works manually but not in Cron
        1.Check absolute paths
        2.script permissions
            chmod +x backup.sh
        3.Environment variables
            Cron does not load your interactive shell environment.
                If your script depends on:
                    DATABASE_URL
                    DJANGO_SETTINGS_MODULE
                    PATH
                    VIRTUAL_ENV
                it may fail.
        4.Python virtual environment
            Instead of: python backup.py
            use: /home/ubuntu/venv/bin/python backup.py
        5.Logging
            0 2 * * * /home/ubuntu/backup.sh >> /var/log/backup.log 2>&1
        6.Test the script manually
            ./backup.sh
        If it fails manually too, the issue isn't Cron.

    











    
    


        
