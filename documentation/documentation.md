
# Documentation

**Derived from material by Anthony Scopatz**

Just like version control and testing, 
documenting your code is the __most important thing__ you can do as a software developer. 
Good documentation is a sublime experience that should permeate your code.

Documentation is important because it is [the only way that 90% of
people will ever interact with you or your code]
(http://pyvideo.org/video/403/pycon-2011--writing-great-documentation).
In fact, it is the only way that scales up; 
there are only so many emails that you can (and want) write.

What is disturbing is that documentation is a forgotten after thought for most developers. 
It turns out that being able to write software _and_
being able to write in your primary spoken language are _different skills_. 
Luckily, we are academics so it is in our nature to publish / write. 
We have no excuse for bad documentation.

## The Many Stages of Documentation

1.  Code Comments
2.  API Documentation
3.  Auto-Documentation
4.  Self-Documenting Code
5.  Readmes
6.  User Guides
7.  Developer Guides



## Code Comments

Every language has a special character (or two) that indicate(s) to the
parser, compiler, or interpreter that whatever comes after or between
these characters should be ignored. 
This allows the author to write, annotate, and explain the code that they are writing *right at the point
that they are writing it!* 
This is especially helpful if something weird, obtuse, or obscure is about to happen because it gives the author
a chance to explain themselves to future developers (often themselves in 1, 2, 6, etc. months).

The best part is that you can put literally *anything* in comments:
publication citations, ASCII art, messages to your friends, and threats to your collaborators.

In Python, the comment character is the hash symbol `#`. 
The following example shows how you might help explain a toaster:


    def toast(slices, toastiness, msg=None):
        
        # make sure the toaster has the right setting
        toastiness = int(toastiness) if 0 < toastiness else 5
    
        print "Engage the bread warming!"
        for slice in slices:
            slice.toast(toastiness)
    
        # log the message, making a default if needed
        if msg is None:
            msg = "Toasted to level {}".format(toastiness)
        logging.info(msg)

However, it is certainly possible to over-document your code with comments. 
Comments should never simply repeat what the code itself is doing. 
The goal is to strike the right balance. 
The appropriate ratio changes with language. 
(Typically higher level languages have greater functionality per line and so have more comments per amount of code.) 
Try to avoid the following:


    # init a to 0
    a = 0
    
    # make b 'a string'
    b = 'a string'
    
    # Add one to a
    a = a + 1
    
    # stopping excessive comments
    self.fall_on_sword()

Note: my personal tendency is to over-comment. 
It doesn't necessarily hurt anything, but what might some detractors be?

## API Documentation

The application programming interface (API) is the definition of the
protocol that two pieces of code may use to interact with one another.
Consider the case of functions. 
All functions have a function signature that specifies how many arguments they accept and their return values.
This signature, along with the module name and function name, is the API.
(The function object/pointer itself is the implementation and is independent of the abstract API.)

Just because you have an argument list, however, does not imply that the
meaning of the arguments is known. 
For example:


    def f(a, b=10):
        
        # ...
        return

We know that `f()` accepts two argument `a` and `b` and that `b` should probably be an integer. 
But what does `f()` actually do? 
What do these arguments mean in this context?

Python allows the user to define API documentation right at the
function, class, module, or variable definition. 
Every Python object may have a `__doc__` attribute that is a string representation of the API docs. 
This is known as a *docstring*.
[PEP257](http://www.python.org/dev/peps/pep-0257/) describes the conventions for docstrings. 
The most important of these is that simple things should have simple docstrings.

If, right below a definition, in the first non-comment, non-whitespace line, there is an unassigned string literal, 
then this string is automatically loaded as the docstring. 
It is this docstring that is then read by the `help()` built-in or the `?` in iPython.


    def mean(numlist):
        """Computes the mean of a list of numbers."""
        
        try:
            total = sum(numlist)
            length = len(numlist)
        except ValueError:
            print "The number list was not a list of numbers."
        except:
            print "There was a problem evaluating the number list."
        return total/length
    
    
    def fib(n):
        """Determines the nth Fibonacci number, where n is 
        a non-negative integer."""
        
        if n < 0 or int(n) != n:
            return NotImplemented
        elif n == 0 or n == 1:
            return n
        else:
            return fib(n - 1) + fib(n - 2)
    
    print help(mean)
    print fib.__doc__

Most Python docstrings are written in a markup language called
[reStructuredText](http://sphinx.pocoo.org/rest.html) (rST). It is
designed to be easy to read, extensible, and provide enough
natural-looking syntax to be able to render nicely. For example, our
toaster docstring might look like:


    def toast(slices, toastiness, msg=None):
        """Toast some bread.
    
        Parameters
        ----------
        slices : sequence of instance of bread slices
            Toast slices to toastiness level
        toastiness : int
            The desired toaster setting
        msg : str, optional
            A message for the toaster's usage log.
    
        """
        
        # make sure the toaster has the right setting
        toastiness = int(toastiness) if 0 < toastiness else 5
    
        print "Engage the bread warming!"
        for slice if slices:
            slice.toast(toastiness)
    
        # log the message, making a default if needed
        if msg is None:
            msg = "Toasted to level {}".format(toastiness)
        logging.info(msg)

## Auto-Documentation

Automatic documentation is the powerful concept that the comments and
docstrings that the developer has already written can be scraped from
the code base and placed on a website or into API documentation. This
significantly reduces the overhead of having to write and maintain many
documents that contain effectively the same information.

Probably the three most popular auto-doc projects are
[javadoc](http://www.oracle.com/technetwork/java/javase/documentation/index-jsp-135444.html)
for Java, [Doxygen](http://www.stack.nl/~dimitri/doxygen/) for most
compiled languages, and [sphinx](http://sphinx.pocoo.org/) for Python.





**Example:** Let's try to make some documentation using Doxygen and the example
file
[`multilevel_solver.py`](https://github.com/rachelslaybaugh/NE24/blob/master/documentation/multilevel_solver.py).

## Exercise

You can build the Doxygen documentation by making a stub Doxygen file for your python code. 
We're going to do this for the
[`JellyBeanCode`](https://github.com/rachelslaybaugh/NE24/blob/master/documentation/EstNumJellies.py)

     doxygen -g doc_example

Now you have a Doxygen template called doc_example. Open the doc_example file. There are many entries you can edit. The ones we're going to worry about here:

 1) Let's change the project name
 
      PROJECT_NAME           = "Jelly Bean Code"
      
 2) Tell Doxygen what files to use
 
      INPUT                  =  "EstNumJellies.py"
      
 3) Tell it to optimize for Python
 
      OPTIMIZE_OUTPUT_JAVA   = YES
 

Information about formatting Doxygen comments for Python can be found [here](http://www.stack.nl/~dimitri/doxygen/manual/docblocks.html#pythonblocks).

Let's build what we've got now and check it out

     doxygen doc_example
     cd html
     google-chrome index.html


Add some additional Doxygen formatted comments to the functions in the `EstNumJellies.py` module. 
Then, using Doxygen, generate a website that auto-documents this module.

Feel free to peruse the multilevel_solver.py code for a more complicated example.

## Self-Documenting Code

Much like in testing where you can simply write perfect code the first
time, there is an analogous philosophy is documentation. This is the
philosophy of [self-documenting
code](http://c2.com/cgi/wiki?SelfDocumentingCode). This ethos makes the
claim that it is often possible to write code in such a way that new
readers can understand what the code does simply by reading it.
Therefore, no extra documentation is required. It is all there in the
code itself.

While there are obvious pitfalls with this approach (assumed knowledge
on the reader's behalf, unavoidable complexities, etc.) there are some
merits. By having meaningful naming conventions and structure it does
become possible to infer a lot about a code just by glancing at it.
Coding standards come from the same desire to have readable software.

However, using this documentation strategy exclusively is *highly* inadvisable.

## Readmes

The omnipresent `README` file is typically a plain text file that sits next to the code. 
They may contain markup, but are often quite terse. 
point of a readme file is to provide only the most basic information to the user / developer.

Readme files are so common that GitHub will render and display the
readme file for all directories whenever you are browsing a source tree.
Even Linux itself has a [readme](https://www.kernel.org/doc/readme/README):

>        Linux kernel release 3.x <http://kernel.org/>
>
> These are the release notes for Linux version 3. Read them carefully,
> as they tell you what this is all about, explain how to install the
> kernel, and what to do if something goes wrong.
>
> WHAT IS LINUX?
>
> Linux is a clone of the operating system Unix, written from scratch
> by Linus Torvalds with assistance from a loosely-knit team of
> hackers across the Net. It aims towards POSIX and Single UNIX
> Specification compliance.
> 
> It has all the features you would expect in a modern fully-fledged
> Unix, including true multitasking, virtual memory, shared libraries,
> demand loading, shared copy-on-write executables, proper memory
> management, and multistack networking including IPv4 and IPv6.
> 
> It is distributed under the GNU General Public License - see the
> accompanying COPYING file for more details.
>
> ...

## User Guides

The next level of documentation are user guides. 
These often take the form of books or pdfs that aim to explain top level architecture and
functionality to (possibly novice) users. 
Such documents are extremely helpful for bringing in new users, 
helping existing users learn more complex features,
going in depth into the theory (math, biology, physics, chemistry, engineering), 
and as a reference manual for advanced users and developers. 
However, because of their high level nature, 
you typically have to wait until the code has stabilized to be able to write a good, comprehensive user's guide.

**Examples:**
[FLASH](http://flash.uchicago.edu/site/flashcode/user_support/flash4b_ug.pdf),
[NumPy](http://docs.scipy.org/doc/numpy/user/).

## Developer Guides

Developer guides are very similar to user guides,
except that they assume a basic mastery of the project. 
They are typically for people who want to *become* developers on a project 
rather than for existing developers. 
They are probably most important for code projects that have plugin architectures and/or
where the line between user and developer is less well defined.

**Examples:** [Android](http://developer.android.com/guide/index.html),
[Python](http://docs.python.org/devguide/).


    
