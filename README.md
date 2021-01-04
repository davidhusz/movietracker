# movietracker

`movietracker` is a small bash script that fetches the latest posts from
[/r/fullmoviesonyoutube](https://www.reddit.com/r/fullmoviesonyoutube/) and
checks whether any of the movies on your personal watchlist are available. It
can be set up to run automatically as a cron job and notify you via email
whenever it finds a movie.

## Configuration

Edit the file `config` to specify how the program should behave. The only
two mandatory arguments are `user_agent` and `watchlist`. This repository
contains an example watchlist, the user agent however has been intentionally
left blank because you need to specify a _unique_ agent yourself. For naming
rules, see [here](https://github.com/reddit-archive/reddit/wiki/API#rules)
(the rules about authentication however do not apply as `movietracker`
does not need a reddit account).

The `logfile` argument defines where debug information should be written to
(can also be `/dev/null`). The default is `/dev/stderr`.

Lastly, you can specify what `movietracker` should do in case it finds matches,
does not find matches, or encounters an error. Each of these arguments should
be a valid bash command because it will be `eval`ed by the program.

`onmatch` defines how to treat matches. The results are made available as
tab-separated values in a file called `matches` (columns: movie title, title
of reddit post, YouTube URL, reddit URL). A version of this as an HTML table
is available as `matches.html`. By default, a pretty-printed version of the
data will be written to stdout.

You could, for example, send yourself an email with the results:

    onmatch="mail -s 'I found some movies for you' \
                  -a 'Content-Type: text/html' \
                  you@yourdomain.com \
                  < matches.html"

`onnomatch` defines what to do in case no matches are found. By default,
nothing is done.

`onerror` defines what to do when the program encounters an error. By default,
it will exit with status code 1.
