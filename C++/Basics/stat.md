  
Managing Files and Directories
---------------------------------

File System Structure: Links, inodes, directories

Working with Directories: Creating, deleting, listing

Advanced Techniques: Monitoring file events

### File System Structure

### stat

`stat(name, buf)`

buf: pointer to a stat structure

`fstat(fd, buf)`

fd: open file descriptor

returns: 0 on success, -1 on failure

##### The stat structure

struct stat {  
   
}
![[stat-structure-1.png]]


The creation time of a file is not recorded

### Demo: Examing File Attributes

### File Types
![[filetype-1.png]]
![[filetype-2.png]]

s: `S_ISUID`

g: `S_ISGID`

t: `S_ISVTX`

owner permissions:

-   r: `S_IRUSR`
    
-   w: `S_IWUSR`
    
-   x: `S_IXUSR`
    
    Group permissions:
    
    -   r: `S_IRGRP`
        
    -   w: `S_IWGRP`
        
    -   x: `S_IXGRP`
        
        Other permissions
        
        -   r: `S_IROTH`
            
        -   w: `S_IWOTH`
            
        -   x: `S_IXOTH`
            
            File Type:
            
            -   `S_ISREG`: 0100000 regular file
                
            -   `S_ISDIR`: 0040000 directory
                
            -   `S_ISCHR`: 0020000 character device
                
            -   `S_ISBLK`: 0060000 block device
                
            -   `S_ISFIFO`: 0010000 FIFO（pipe）
                
            -   `S_ISLNK`: 0120000 symbolic link
                
            
            ​  
            #include <sys/stat.h>  
            if (statbuf.st\_mode & S\_IWOTH)  
             printf("file is world writeable");  
            ​
            
            ​  
            if (S\_ISREG(statbuf.st\_mode))  
             printf("regular file");  
            ​
            
            打印文件类型
            
            ​  
            printf("file type: %s\\n", filetype\[(sb.st\_mode \>> 12) & 017\]);  
            ​
            
            ### Links and Symbolic Links
            
            open(name, flags, mode)
            
            create(name, mode)
            
            #### creating removing links
            
            `link(oldname, newname)`
            
            Give the file an additional name
            
            `unlink(name)`
            
            If this is the last remaining link the file is deleted.
            
            Deletion is postponed if a process has the file open
            
            #### Symbolic Links
            
            -   A symbolic link is a file that contains the name of another file
                
                -   Similar to a shortcut on Windows
                    
            -   Symboloc links can do things that hard links cannot
                
                -   Link to Directories
                    
                -   Link across file system boundaries
                    
            
            ​  
            symlink(oldname, newname);  
            ​  
            unlink(name);  
            // removes the symlink not the target file
            ![[link-1.png]]
			![[link-2.png]]
            
            ### Directory Traversal
            
            ​  
            #include <stdio.h>  
            #include <sys/stat.h>  
            #include <dirent.h>  
            ​  
            void main()  
            {  
             DIR \*d;  
             struct dirent \*info; // A directory entry  
             struct stat sb;  
             long total \= 0;  
               
             d \= opendir(".");  
               
             while ((info \= readdir(d)) != NULL) {  
             stat(info\->d\_name, &sb);  
             total += sb.st\_size;  
             }  
             closedir(d);  
             printf("total size = %ld\\n", total);  
            }  
            ​
            
            ### Monitoring file system events
            
            The inotify API
            
            Creating an inotify instance
            
            Adding to the watch list
            
            Reading events
            
            #### File system events
            
            individual files or whole directories can be watched
            
            not recursive
            
            Create inotify instance (file descriptor)
            
            \=>
            
            Add watch item
            
            \=>
            
            Read and process events
            
            ##### Creating inotify instance
            
            `fd = inotify_init()`
            
            ##### Add watch item
            
            `wd = inotify_add_watch(fd, path, mask)`
            
            -   returns a watch descriptor (small integer) identifying this watch
                
            -   `fd`: File descriptor of inotify instance
                
            -   `path`: Name of file or direcotry to be watched
                
            -   `mask`: Bit mask specifying the events to be monitored
                
            