# Disable redirects in Firefox

Sometimes you need to analyse the behaviour of a website regarding redirects.

* To temporarily disable redirects (301, 302,...) for a specific site in Firefox we must:

1.- Go to history (Ctrl + h), type the full URL of the site you want (e.g. http://google.com) and right click over the name.

2.- Choose "Forget about this site" to remove it from history. Please be patient: if you have a huge history regarding the site, this can take a while and Firefox may seem to freeze (it hasn't).

3.- Open the advanced settings: In the addess bar type "about:config", read the disclaimer and click the "I accept the risk".

4.- Inside the advanced settings, search and change the following entries:

* accessibility.blockautorefresh -> set to true (default is false)
* network.http.redirection-limit -> set to 0 (default is 20)

5.- Now when you load the URL any redirection will fail and you will stay in the original site (usually with an error stating "The page isn’t redirecting properly").


6.- To restore the redirections, go back to the advanced settings and restore the default values for the two entries.
