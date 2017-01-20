(please view with raw format)

1. What's the use of do while(0) when we define a macro?
 
http://stackoverflow.com/questions/154136/do-while-and-if-else-statements-in-c-c-macros

2. How to build & install linux kernel?
---think pad E531, virtual box 5.0.10, ubuntu 14.04---

sudo apt-get install dpkg-dev
sudo apt-get install build-essential
sudo apt-get install kernel-package 
sudo apt-get install libncurses5 libncurses5-dev 

# 'dpkg -l  |grep linux-image' get current image version
mkdir 3.16.0-30-generic
cd 3.16.0-30-generic
apt-get source linux-image-$(uname -r) 
# 'make kernelversion' get src version
sudo apt-get build-dep linux-image-$(uname -r) 

cp /boot/config-XX  ./.config
make oldconfig
make menuconfig  
make-kpkg  --initrd --revision 1116  --append-to-version -20151116  kernel_image
install the kernel deb file, reboot

also refer to : 
http://www.cnblogs.com/wwang/archive/2011/01/07/1929486.html
http://blog.csdn.net/xin_yu_xin/article/details/42184899

3. refer to https://gcc.gnu.org/onlinedocs/cpp/Variadic-Macros.html
3.1 use variable argument list withinin function:
	static int printf(const char *fmt, ...)
	{
	    va_list args;
	    int i;
	 
	    va_start(args, fmt);
	    i=vsprintf(printbuf, fmt, args);
	    va_end(args);
	    return i;
	}
	
3.2 use variable argument list in macro:
	#define DEBUG(level, fmt...) do {               \
        if (g_debuglevel >= level) {               \
                fprintf(stdout, fmt);		\
                fflush(stdout);                 \
        }                                       \
} while (0)

#define DEBUG(level, fmt, ...)						\
	do {								\
		FILE *fp = test->ofile? test->ofile : stdout;		\
		if (test->verbose >= level) {				\
			fprintf(fp, "Test: %15s:%-10s       " fmt,	\
			test->major, test->minor, ## __VA_ARGS__);	\
			fflush(stdout);					\
		}							\
	} while(0)
	
4. What's the difference between inline function vs macro?
Inline replaces a call to a function with the body of the function, 
however, inline is just a request to the compiler that could be ignored 
(you could still pass some flags to the compiler to force inline or 
use *always_inline* attribute with gcc).
A macro on the other hand, is expanded by the preprocessor before compilation, 
so it's just like text substitution, also macros are not type checked, 
inline functions are.

5. From c file to exe file
a. Preprocess: gcc -E  helloword.c -o helloword.i (cpp)
b. Compile: gcc -S helloword.i -o helloword.s (cc1)
c. Assemble: gcc -c helloword.s -o helloword.o (as)
d. Link:  gcc helloword.o -o helloword (ld)
e. Run: ./helloword
Hello world

5. Write reentrant and thread-safe code
http://www.cnblogs.com/safeking/archive/2007/03/09/668873.html

6. http://stackoverflow.com/questions/12469836/how-to-write-system-calls-on-debian-ubuntu

7. macro container_of in linux kernel source code:
#define container_of(ptr, type, member) ({    \
     const typeof ( ((type *)0) ->member ) *__mptr = (ptr);    \
     (type *)((char *)__mptr - offsetof(type, member));}) 

8. Difference of sync/async, blocking/nonblocking
http://blog.csdn.net/hguisu/article/details/7453390

9. Given a string which contains only lowercase letters, remove duplicate letters so that every letter appear once and only once. You must make sure your result is the smallest in lexicographical order among all possible results.
The simplest python code of others, which I liked very much:
    def removeDuplicateLetters(self, s):
        rindex = {c: i for i, c in enumerate(s)}
        result = ''
        for i, c in enumerate(s):
            if c not in result:
                while c < result[-1:] and i < rindex[result[-1]]:
                    result = result[:-1]
                result += c
        return result
        
10. GDB daily use
gcc(g++) -ggdb3 -o execfilename srcfile.c(srcfile.cpp); gdb execfilename
a. source code browsing
list <linenum>, list <filename:linenum> 
list <function>, list <filename:function>
set listsize <count>
search <regexp>, reverse-search <regexp>
info line <linenum>
info line <filename:function>
disassemble, disassemble func

b. break points
break <linenum> thread <threadno>, break *address, b xxx
info b, i b
clear <filename:linenum> , d [range]

c. debugging
continue/c, s, n
s/n

d. run time value check
print /<f> <expr>
examine/<n/f/u>  <addr> 
display/<fmt> <expr> # set auto display
bt, info f, info locals
f <n>, info r, thread <threadno>

11. return the sum of Primes within 100
>>import math
>>sum([x for x in range ( 1 , 100 ) if not [y for y in range ( 2 ,int(math.sqrt(x))+1) if x % y == 0 ]])
>>1061

12. use awk to print the same first column in two files:
awk -F, 'BEGIN{aa[0]=1} {if (FILENAME=="a")  aa[$1]=$2; else if (FILENAME=="b" && aa[$1]) print $1} ' a b

