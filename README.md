Download Link: https://assignmentchef.com/product/solved-cmsc216-project-4
<br>
In this project you will use dynamic memory allocation techniques in order to implement a calendar application. There are two deadlines associated with this project:

<ul>

 <li>Mon, Jul 13, 11:55 PM – Your code must pass the first two public tests (public01, public02). That is the only requirement for this deadline. We will not grade the code for style. This first part is worth .5% of your course grade (NOT .5% of this project grade). Notice you can still submit late for this part.</li>

 <li>Wed, Jul 15, 11:55 PM – Final deadline for the project. Notice you can still submit late (as usual).</li>

</ul>

Remember that every project has a good faith attempt. The good faith attempt for this project will be posted later on.

<strong>2      Objectives</strong>

To practice dynamic memory allocation, function pointers and linked lists.

<h1>3      Project Files</h1>

The files for this project can be found in the project4 directory of the public class directory. Make sure you copy that folder to the 216 folder in your home directory. The files associated with the distribution are:

<ol>

 <li>h – This file provides prototypes for the functions your must implement. It also provides structure definitions, typedefs, and symbolic constants. <strong>You may not modify this file.</strong></li>

 <li>h – Defines an Event structure. <strong>You may not modify this file.</strong></li>

 <li>.h and .c files associated with the memory checker tool.</li>

 <li>Makefile – This is the file where you will define the project’s makefile. The name of the makefile must start with capital M.</li>

 <li>c, public02.c, public03.c, public04.c, public05.c, public06.c – The public tests for this project.</li>

 <li>student tests.c – You will write student tests in this file.</li>

</ol>

<h1>4      Specifications</h1>

<h2>4.1     Makefile</h2>

Your Makefile should be set up so that all programs are built using separate compilation (i.e., source files are turned into object files, which are then linked together in a separate step). All object files should be built using the following options:

-ansi -Wall -g -O0 -Wwrite-strings -Wshadow -pedantic-errors -fstack-protector-all -Wextra

IMPORTANT: <strong>You must implement the Makefile using explicit rules</strong>; using implicit rules is not allowed. The Makefile examples associated with the lecture <a href="http://www.cs.umd.edu/class/summer2020/cmsc216/lectures/Week4/Make.pdf">http://www.cs.umd.edu/class/summer2020/cmsc216/lectures/Week4/Make.pdf</a>

rely on explicit rules. You should define your Makefile from the start; if you decide to write your Makefile at the end, make sure you back up your code before you write the Makefile. Every semester we have students that by mistake delete their implementation due to a Makefile error.

You will need to have the following targets present in your Makefile:

<ol>

 <li>all: make all executables</li>

 <li>public01, public02, public03, public04, public05, public06: each one building a public test</li>

 <li>student_tests: one of your executables.</li>

 <li>o:</li>

 <li>o:</li>

 <li>o:</li>

 <li>clean: delete all object files and executables</li>

</ol>

While you are welcome to add other targets to your Makefile, the ones listed above must be present. Make sure you add all the necessary targets that avoid any unnecessary compilation (hint: use the touch command to check your makefile).

<h2>4.2      Calendar Overview</h2>

For this project you will implement a calendar application that allows the scheduling of events at specific days. A calendar is defined by the following structures:

typedef struct event { char *name; int start_time, duration_minutes; void *info; struct event *next;

} Event;

typedef struct calendar { char *name; Event **events; int days, total_events; int (*comp_func) (const void *ptr1, const void *ptr2); void (*free_info_func) (void *ptr);

} Calendar;

An Event structure will represent a node in a linked list. The Event structure provides an event’s name, start time (military time), duration in minutes, a pointer to data that provides information about the event, and a reference to another Event structure.

The Calendar structure keeps track of events by using an array of linked lists (<strong>events </strong>field). Each array entry is associated with a linked list that represents events scheduled for a particular day. The size of the array corresponds to the number of days the calendar has.

Calendar events can be sorted based on different criteria. The <strong>comp func </strong>field represents a comparison function (similar to compareTo in Java) that will allow the insertion of events based on the criteria defined by the function. For example, you can have a calendar where events are sorted by duration, and another where events are sorted by name.

Events can be associated with additional information (<strong>info </strong>field). This information can be a structure, a string, anything. Once an event is no longer needed, the calendar can remove this information if a free function (that knows how to free anything associated with the information) is provided. The <strong>free info func </strong>field represents this free function.

<h2>4.3      Calendar Functions</h2>

You must write all your functions in a file called calendar.c. Functions rely on the macros SUCCESS and FAILURE defined in calendar.h.

<ol>

 <li>int init_calendar(const char *name, int days, int (*comp_func) (const void *ptr1, const void *ptr2), void (*free_info_func) (void *ptr), Calendar ** calendar);</li>

</ol>

This function initializes a Calendar structure based on the parameters provided. The function allocates memory for the following items:

<ol>

 <li>Calendar structure</li>

 <li><strong>name </strong>field will be assigned memory necessary to store a copy of the name parameter. The name represents the calendar’s name.</li>

 <li><strong>events </strong>field will be assigned memory necessary to represent an array of pointers to Event structures. The size of this array corresponds to the <strong>days </strong></li>

</ol>

The out parameter <strong>calendar </strong>will provide access to the new Calendar structure. Notice the total number of events will be set to zero. The function will fail and return FAILURE if calendar and/or name are NULL, if the number of days is less than 1, or if any memory allocation fails. Otherwise the function will return SUCCESS.

<ol start="2">

 <li>int print_calendar(Calendar *calendar, FILE *output_stream, int print_all);</li>

</ol>

This function prints, to the designated output stream, the calendar’s name, days, and total number of events if <strong>print all </strong>is true; otherwise this information will not be printed. Information about each event (name, start time, and duration) will be printed regardless the value of <strong>print all</strong>. See public tests output for format information. Notice that the heading ”**** Events ****” will always be printed.

This function will fail and return FAILURE if calendar and/or output stream are NULL; otherwise the function will return SUCCESS.

<ol start="3">

 <li>int add_event(Calendar *calendar, const char *name, int start_time, int duration_minutes, void *info, int day);</li>

</ol>

This function adds the specified event to the list associated with the <strong>day </strong>parameter. The event must be added in increasing sorted order using the comparison function (comp func) that allows comparison of two events. The function must allocate memory for the new event and for the event’s name. Other fields of the event structure will be initialized based on the parameter values.

This function will fail and return FAILURE if calendar and/or name are NULL, start time is invalid (must be a value between 0 and 2400, inclusive), duration minutes is less than or equal to 0, day is less than 1 or greater than the number of calendar days, if the event already exist, or if any memory allocation fails. Otherwise the function will return SUCCESS.

<strong>Notice that events are uniquely identified across the calendar by name.</strong>

<ol start="4">

 <li>int find_event(Calendar *calendar, const char *name, Event **event);</li>

</ol>

This function will return a pointer (via the out parameter <strong>event</strong>) to the event with the specified <strong>name </strong>(if any). If the <strong>event </strong>parameter is NULL, no pointer will be returned. Notice the out parameter <strong>event </strong>should not be modified unless the event is found. The function will fail and return FAILURE if calendar and/or name are NULL. The function will return SUCCESS if the event was found and FAILURE otherwise.

<ol start="5">

 <li>int find_event_in_day(Calendar *calendar, const char *name, int day, Event **event);</li>

</ol>

This function will return a pointer (via the out parameter <strong>event</strong>) to the event with the specified <strong>name </strong>in the specified <strong>day </strong>(if such event exist). If the <strong>event </strong>parameter is NULL, no pointer will be returned. Notice the out parameter <strong>event </strong>should not be modified unless the event is found. The function will fail and return FAILURE if calendar and/or name are NULL, or if the day parameter is less than 1 or greater than the number of calendar days. The function will return SUCCESS if the event was found and FAILURE otherwise.

<ol start="6">

 <li>int remove_event(Calendar *calendar, const char *name);</li>

</ol>

This function will remove the specified event from the calendar returning any memory allocated for the event. If the event has an <strong>info </strong>field other than NULL and a <strong>free info func </strong>exists, the function will be called on the <strong>info </strong>field. The number of calendar events must be adjusted accordingly. This function will fail and return FAILURE if calendar and/or name are NULL or if the event cannot be found; otherwise the function will return SUCCESS.

<ol start="7">

 <li>void *get_event_info(Calendar *calendar, const char *name);</li>

</ol>

This function returns the <strong>info </strong>pointer associated with the specified event. The function returns NULL if the event is not found. You can assume the <strong>calendar </strong>and <strong>name </strong>parameters are different than NULL.

<ol start="8">

 <li>int clear_calendar(Calendar *calendar);</li>

</ol>

This function will remove all the event lists associated with the calendar and set them to empty lists. Notice that the array holding the event lists will not be removed. The total number of events will be set to 0. If an event has an <strong>info </strong>field other than NULL and a <strong>free info func </strong>exists, the function will be called on the <strong>info </strong>field. This function will fail and return FAILURE if calendar is NULL; otherwise the function will return SUCCESS.

<ol start="9">

 <li>int clear_day(Calendar *calendar, int day);</li>

</ol>

This function will remove all the events for the specified <strong>day </strong>setting the event list to empty. The total number of events will be adjusted accordingly. If an event has an <strong>info </strong>field other than NULL and a <strong>free info func </strong>exists, the function will be called on the <strong>info </strong>field. This function will fail and return FAILURE if calendar is NULL, or if the day is less than 1 or greater than the calendar days; otherwise the function will return SUCCESS.

<ol start="10">

 <li>int destroy_calendar(Calendar *calendar);</li>

</ol>

This function will return memory that was dynamically-allocated for the calendar. If an event has an <strong>info </strong>field other than NULL and a <strong>free info func </strong>exists, the function will be called on the <strong>info </strong>field. This function will fail and return FAILURE if calendar is NULL; otherwise the function will return SUCCESS.

<h2>4.4      Calendar Testing</h2>

<ol>

 <li>You must define tests for your calendar implementation as described in the file student tests.c. You will be graded on the quality of these tests. The main function in student tests should run all tests, but exit with the EXIT FAILURE code if any of your tests fails, and EXIT SUCCESS otherwise. The student tests.c file in the distribution performs this task, though you are welcome to rewrite it as you add tests.</li>

 <li>You should write student tests as you develop your code. If you need assistance during office hours, we may ask for these tests.</li>

 <li>We expected at least one test for each function you need to implement.</li>

 <li>We expect your tests to cover specific cases. For example, your tests should check what takes place when adding the same event twice. If we do not see such a test case you will lose credit. There are other cases we are expecting you to test.</li>

</ol>

<h2>4.5       Dynamic Memory Allocation</h2>

<ol>

 <li>As with all programs that use dynamic memory allocation, you must avoid memory leaks in your code, in all circumstances.</li>

 <li>Keep in mind that when you run valgrind on code that uses the memory tool we have provided, the tool may detect leaks for memory that valgrind allocates. To test your code using valgrind, disable our memory tool (by commenting out the function calls associated with the tool) and run valgrind. While running valgrind use the suggested options (e.g., valgrind –leak-check=full).</li>

 <li>If a function requires multiple dynamic-memory allocations and one of them fails (malloc/callocs returns NULL), you are not responsible for freeing the memory of those allocations that were successful. For example:</li>

</ol>

mem1 = malloc(…) /* This one is successful */

mem2 = malloc(…) /* This one failed, just return FAILURE (don’t worry about mem1) */

<ol start="4">

 <li>The best way to check you are using dynamic-memory allocation correctly is to test often.</li>

</ol>