# The Sun Network Filesystem: Design, Implementation and Experience

## the 3 most important things

* NFS uses a statless protocol

* NFS uses External Data Representation (XDR) specification to describe
  protocols in a machine and system independent way.

## design goals

* machine and operating system independence
* crash recovery
* transparent access
* UNIX semantics maintained on UNIX client
* reasonable performance

## quote
>In order to build NFS into the UNIX kernel in a way that is transparent to applications,
we decided to add a new interface to the kernel which separates generic filesystem operations
from specific filesystem implementations. The “filesystem interface” consists of two parts:
the Virtual File System (VFS) interface defines the operations that can be done on a filesystem,
while the virtual node (vnode) interface defines the operations that can be done on a file within
that filesystem. This new interface allows us to implement and install new filesystems
in much the same way as new device drivers are added to the kernel.

>The NFS protocol and RPC are built on top of the SUN External Data Representation (XDR)
specification. XDR defines the size, byte order and alignment of basic data types such
as string, integer, union, boolean and array. Complex structures can be built from the basic
XDR data types. Using XDR not only makes protocols machine and language independent, it also make
them easy to define.

>The most common NFS procedure parameter is a structure called a file handle (fhandle or fh)
which is provided by the server and used by the client to reference a file.
The fhandle is opaque, that is, the client never looks at the contents of the fhandle,
but uses it when operations are done on that file.

>These extra numbers make it possible for the server to use the inode number, inode generation number,
and filesystem id together as the fhandle for a file. The inode generation number is necessary
because the server may hand out an fhandle with an inode number of a file
that is later removed and the inode reused.

>Rather than doing a "late binding" of file address, we decided to do the hostname lookup and file address
binding once per filesystem by allowing the client to attach a remote filesystem to a directory with the
mount command.

>In the NFS filesystem, passing whole pathnames would force the server to keep track of
all of the mount points of its clients in order to determine where to break the pathname
and this would violate server statelessness. The inefficiency of looking up one component
at a time can be alleviated with a cache of directory vnodes.

>Most of the work in adding the new interface was in finding and fixing all of
the places in the kernel that used inodes directly, and code that contained
implicit knowledge of inodes or disk layout.

>For example, NFS allows two flavors of mount, soft and hard. A hard mounted
filesystem will retry NFS requests forever if the server goes down,
while a soft mount gives up after a while and returns an error.
The problem with soft mounts is that most UNIX programs are not very good about
checking return status from system calls so you can get some strange behavior
when servers go down.

>NFS does not support remote file locking. We purposely did not include this
as part of the protocol because we could not find a set of file locking facilities
that everyone agrees is correct. Instead we have a separate, RPC based file locking
facility. Because file locking is an inherently stateful service, the lock service
depends on yet another RPC based service called the status monitor.

>The problem with this authentication method is that the mapping from uid and gid
to user must be the same on the server and client. This implies a flat uid, gid
space over a whole local network. This is not acceptable in the long run.

>What we did to make open file removal work on remote files was check in the client
VFS remove operation if the file is open, and if so rename it instead of removing it.
This makes it (sort of) invisible to the client and still allows reading and writing.
The client kernel then removes the new name when the vnode becomes inactive. We call
this the 3/4 solution because if the client crashes between the rename and remove a
garbage file is left on the server.

>Much of the development time of NFS has been spent in improving performance.
Our goal was to make NFS comparable in speed to a small local disk The speed
we were interested in is not raw throughput, but how long it takes to do normal work.

>To improve the performance of NFS, we implemented the usual read-ahead and
write-behind buffer caches on both the client and server sides. We also added
caches on the client side for file attributes and directory names.

>RFS uses streams 7 to hide the details of underlying protocols. This should
make it easy to plug in new transport protocols. Unfortunately, RFS uses the
virtual circuit connection of the transport protocol to detect server and
client crashes. (The exclusive use of transport properties to drive session
semantics is a common design flaw in many network applications)

## more

* why NFS sucks

