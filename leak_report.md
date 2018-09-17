# Leak report

The issue that is occuring is that in the strip function calloc is being called which
is allocating space for the saved characters and the null terminator. A good thing to note
about this is that if the string is only whitespace it will not allocate space so we do
not need to free up space in those cases. We cannot free up the space in strip because
the value being returned uses the allocated space. This means we have to free up space in
is_clean which calls strip. This must take place after strcmp(str, cleaned) is called so we
do not free up the space too early and before the return so it gets called. We must use the note 
from earlier here, if the string is all whitespace, strip will return an empty string so we have 
to make sure not to try and free up space when cleaned is an empty string. So if the length of 
cleaned does not equal 0 we have to free up space.
This fix starts on line 69 and ends on line 74.
