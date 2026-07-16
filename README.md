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

    


        
