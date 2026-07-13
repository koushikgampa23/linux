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

### Ls and cd commands
    ls - Shows files in the current directory
    cd - change directory
    cd .. - Go back one directory
    cd ~ - Go to home directory
    cd / - Go to filesystem root

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

    


    
