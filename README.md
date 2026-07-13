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
    


    
    
    
