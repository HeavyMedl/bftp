# bftp 
<<<<<<< HEAD
A bash script with ftp mirroring functionality that wraps cURL as a platform for **Explicit FTP over SSL (FTPES)**. A command line ftp client.
=======
A command line **Explicit FTP over SSL (FTPES)** cURL wrapper that mirrors remote files locally. 
>>>>>>> 032d41ec9a0c3e488188c8ea4d39102a294b6d45

------
#### Purpose
The motivation behind bftp was to learn bash programming.  When I started using a seedbox (private server) to upload and download files, I quickly grew tired of reopening filezilla for my needs.  I wanted to automate FTP over SSL using bash from my terminal (OSX) using only cURL.  This was a perfect oportunity to accomplish both tasks.

======
#### Features
- **FTPES** - Because bftp wraps cURL, we can utilize its robust features.  Bftp becomes a **Explicit FTP over SSL (FTPES)** client by using cURL's `--ssl-reqd` flag.  This flag requires SSL/TLS for the connection. The connection terminates without it.  This means the data stream transfer is encrypted!

- **Mirror files and directories** - Bftp recursively creates a local file structure that *mirrors* your remote file structure.  Meaning if you execute a bftp command to get remote files, bftp prefixes a namespace and replicates the file structure from your ftp server to your local.
 
<<<<<<< HEAD
	`hostname.com:port/**root/myfiles/[foo.txt, bar.txt]**`

	becomes

	`$HOME/Desktop/**bftp.downloads/root/myfiles/[foo.txt, bar.txt]**`
=======
	`hostname.com:port/root/myfiles/[foo.txt, bar.txt]`
k
	becomes

	`$HOME/Desktop/bftp.downloads/root/myfiles/[foo.txt, bar.txt]`
>>>>>>> 032d41ec9a0c3e488188c8ea4d39102a294b6d45

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
<<<<<<< HEAD
=======
> **Note:**

>>>>>>> 032d41ec9a0c3e488188c8ea4d39102a294b6d45
> If you manually copy/paste the script, you'll need to `chmod -x bftp` to make the script executable.

Or use the git route and clone the repository with this command
```bash
<<<<<<< HEAD
$ git clone https://github.com/kurtmedley/bftp.git
=======
$ git clone https://github.com/kurtquake/bftp.git
>>>>>>> 032d41ec9a0c3e488188c8ea4d39102a294b6d45
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
<<<<<<< HEAD
Now that you have the script ready to run, you need to set endpoints to your ftpserver.  By default, the endpoints have place holders in them
=======
Now that you have the script ready to run, you need to set endpoints to your ftp server.  By default, the endpoints have place holders in them because you're not allowed to access my seedbox :P Let's configure bftp to use your ftp server.  Identify the root folder where your files reside.  In my seedbox, my root folder is `/rtorrent/downloads/`.  We're using the ftp protocol in cURL, so our fully qualified endpoint would be something like this:

`ftp://user:password@nad.seedstuff.ca:9999/rtorrent/downloads/`

Open the script with a text editor.

```bash
$ vim bash 
```
Find the two variables `USERDIR` and `FTPQUALIFIEDURL` near the top of the script:

```bash
USERDIR="/my/local/directory/"
FTPQUALIFIEDURL="ftp://user:password@nad.seedstuff.ca:9999/rtorrent/downloads/"
```

`USERDIR` represents the qualified path to your local directory.  This is where bftp will place remote file content

`FTPQUALIFIEDURL` represents the qualified path to your remote files.

> **Note**:

> Observe the syntax of these paths. Note the placement of the forward slashes (`/`) as these are important.  Also, make sure to replace `user` and `password` with your unique credentials

Done! Now you can run `bftp` from your command line.  Read on for a command listing and usage examples.

=====
#### Command List and Usage

1. **view**
	
	Description:

	Lists all contents at your designated remote file path as an ordered list.

	Usage:

	```bash
	$ bftp view
	```

	Output:

	```bash
	Fetching contents...
	1 - ubuntu-14.04-desktop-amd64.iso
	2 - ubuntu-14.04-desktop-i386.iso
	$ 
	```


2. **get-one** [*my/local/path/*]
	
	Description: 

	Get a single file/directory from the remote server.  This command will display an ordered list of contents at your designated remote file path.  Enter a number corresponding to the file you'd like to download and bftp will mirror it into your local directory.  bftp mirrors the names of the remote files into your local directory.  If [*my/local/path/*] is included, bftp will use this as the target directory, otherwise it defaults to `USERDIR`.  `get-one` will skip your selection if your target directory contains exactly the same named file/directory.

	Usage:

	```bash
	$ bftp get-one
	```

	Output:

	```bash
	Fetching contents...
	1 - ubuntu-14.04-desktop-amd64.iso
	2 - ubuntu-14.04-desktop-i386.iso
	Type a number corresponding to the file number you'd like to download and hit [Enter]: 1
	Using /Users/kurtmedley/Desktop/
	--------------------
	Downloading < ubuntu-14.04-desktop-amd64.iso > to /Users/kurtmedley/Desktop
	--------------------
	  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
	                                 Dload  Upload   Total   Spent    Left  Speed
	  1  964M    1 11.5M    0     0   592k      0  0:27:47  0:00:20  0:27:27  677k
	```
	
	> **Note**:

	> `USERDIR` is defined as `/Users/kurtmedley/Desktop/`

	Usage:

	```bash
	$ bftp get-one /Users/kurtmedley/Downloads/
	```

	Output:

	```bash
	Fetching contents...
	1 - ubuntu-14.04-desktop-amd64.iso
	2 - ubuntu-14.04-desktop-i386.iso
	Type a number corresponding to the file number you'd like to download and hit [Enter]: 2
	Creating and entering /Users/kurtmedley/Downloads//bftp.downloads/
	--------------------
	Downloading < ubuntu-14.04-desktop-i386.iso > to /Users/kurtmedley/Downloads/bftp.downloads
	--------------------
	  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
	                                 Dload  Upload   Total   Spent    Left  Speed
	  2  970M    2 26.0M    0     0   558k      0  0:29:38  0:00:47  0:28:51  613k
	```

	> **Note**:

	> `USERDIR` may be defined, but the file path you pass as a parameter takes precedent.  bftp then creates a namespace directory inside your designated path called `bftp.downloads`.

3. **get-all** [*my/local/path/*]

	Description:

	Get all the files/directories from the remote server.  This command will sequentially download all the contents at your designated remote file path.  bftp mirrors the names of the remote files into your local directory.  If [*my/local/path/*] is included, bftp will use this as the target directory, otherwise it defaults to `USERDIR`.  `get-all` will skip your selection if your target directory contains exactly the same named file/directory.  If you choose to automate this script, this feature becomes desirable as you won't overwrite files/directories.

	Usage:

	```bash
	$ bftp get-all
	```

	Output:

	```bash
	Using /Users/kurtmedley/Desktop/

	Downloading 2 items from ftp://user:password@nad.seedstuff.ca:9999/rtorrent/downloads/ to /Users/kurtmedley/Desktop/

	--------------------
	Downloading < ubuntu-14.04-desktop-amd64.iso > to /Users/kurtmedley/Desktop
	--------------------
	  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
	                                 Dload  Upload   Total   Spent    Left  Speed
	  0  964M    0 8912k    0     0   516k      0  0:31:50  0:00:17  0:31:33  551k
	```

	> **Note**:

	> `USERDIR` is defined as `/Users/kurtmedley/Desktop/`

	Usage:

	```bash
	$ bftp get-all my/local/dir/
	```

	Output:

	```bash
	Creating and entering /Users/kurtmedley/Downloads//bftp.downloads/

	Downloading 2 items from ftp://user:password@nad.seedstuff.ca:9999/rtorrent/downloads/ to /Users/kurtmedley/Downloads//bftp.downloads/

	--------------------
	Downloading < ubuntu-14.04-desktop-amd64.iso > to /Users/kurtmedley/Downloads/bftp.downloads
	--------------------
	  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
	                                 Dload  Upload   Total   Spent    Left  Speed
	  1  964M    1 11.8M    0     0   471k      0  0:34:51  0:00:25  0:34:26  614k
	```
	
	> **Note**:

	> `USERDIR` may be defined, but the file path you pass as a parameter takes precedent.  bftp then creates a namespace directory inside your designated path called `bftp.downloads`.
>>>>>>> 032d41ec9a0c3e488188c8ea4d39102a294b6d45
