(please view with raw format)

1. What's the use of do while(0) when we define a macro?
 
http://stackoverflow.com/questions/154136/do-while-and-if-else-statements-in-c-c-macros

2. How to build & install linux kernel?
 
mkdir 3.16.0-30-generic
cd 3.16.0-30-generic
sudo apt-get install dpkg-dev
apt-get source linux-image-$(uname -r) 
sudo apt-get install build-essential
sudo apt-get install kernel-package
sudo apt-get build-dep linux-image-$(uname -r)  
make oldconfig
sudo apt-get install libncurses5 libncurses5-dev 
make menuconfig  
make-kpkg
make
also refer to : http://blog.csdn.net/xin_yu_xin/article/details/42184899

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

