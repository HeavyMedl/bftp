# bftp 
A bash script with ftp mirroring functionality that wraps cURL as a platform for **Explicit FTP over SSL (FTPES)**. A command line ftp client.

------
#### Purpose
The motivation behind bftp was to learn bash programming.  When I started using a seedbox (private server) to upload and download files, I quickly grew tired of reopening filezilla for my needs.  I wanted to automate FTP over SSL using bash from my terminal (OSX) using only cURL.  This was a perfect oportunity to accomplish both tasks.

======
#### Features
- **FTPES** - Because bftp wraps cURL, we can utilize its robust features.  Bftp becomes a **Explicit FTP over SSL (FTPES)** client by using cURL's `--ssl-reqd` flag.  This flag requires SSL/TLS for the connection. The connection terminates without it.  This means the data stream transfer is encrypted!

- **Mirror files and directories** - Bftp recursively creates a local file structure that *mirrors* your remote file structure.  Meaning if you execute a bftp command to get remote files, bftp prefixes a namespace and replicates the file structure from your ftp server to your local.
 
	`hostname.com:port/**root/myfiles/[foo.txt, bar.txt]**`

	becomes

	`$HOME/Desktop/**bftp.downloads/root/myfiles/[foo.txt, bar.txt]**`

	> **Note**:

	> If a default directory is specified in your configuration, bftp does not create a namespace in that directory, it simply mirrors your remote files into that directory.
  
- **Automate** - Modify `crontab` or a job scheduler to utilize bftp as means of getting all of your remote files using `$ bftp get-all`. OR keeping in the spirit of wrapping shell executables, wrap bftp in your own process.

- **Operations** - *some operations are still in development*
  - Get single or multiple remote files/directories to local  **functional**
  - Put single or multiple local files/directories to remote  **in development**
  - Delete single or multiple remote files/directores         **in development**

=====
#### Setup
Since this is literally just a bash script, you are welcome to simply create a new file with a text editor, copy and paste the script contents, and save it to your desired path.
```bash
$ cd /usr/local/bin/ && vim bftp # paste script in this file
```
> If you manually copy/paste the script, you'll need to `chmod -x bftp` to make the script executable.

Or use the git route and clone the repository with this command
```bash
$ git clone https://github.com/kurtmedley/bftp.git
```

##### Dependencies
As this is a cURL wrapper, it is the only depedency.  If for whatever reason you don't have the executable, get it here: <http://curl.haxx.se/download.html>

##### Running the script
You can run the script by navigating to the directory you saved it to and typing
```bash
$ ./bftp
```
Alternatively, you can place `bftp` in a directory that is included in your `$PATH` variable.  This will allow you to run the script without the `./` syntax and from any path you may `cd` to. Find out what directories are in the `$PATH` variable
```bash
$ $PATH
-bash: /usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin ...
$ mv bftp /usr/local/bin # move bftp into a directory defined in your $PATH variable
```
You can append a new directory to the `$PATH` variable by running this
```bash
$ export PATH=$PATH:/new/path
```
Now place `bftp` in `/new/path`
If you're using OSX, the most convenient way to do this is to append your new path to your `/etc/paths` file
```bash
$ sudo vim /etc/paths
/usr/bin
/bin
/usr/sbin
/sbin
/usr/local/bin
/new/path/ # use your editor to place your new path here
```
Now everytime you restart your terminal, your new path will be defined in your `$PATH` variable.

##### Configuration
Now that you have the script ready to run, you need to set endpoints to your ftpserver.  By default, the endpoints have place holders in them
