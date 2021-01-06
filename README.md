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

The `logfile` argument defines the file that debug information and errors
should be written to (can also be a pseudofile like `/dev/null`). The default
is `log`.

Lastly, you can specify what `movietracker` should do in case it finds matches,
does not find matches, or encounters an error. Each of the following arguments
should be a valid bash command because it will be `eval`ed by the program.

`onmatch` defines how to treat matches. The results are made available as
tab-separated values in a file called `matches` (columns: movie title, title of
reddit post, YouTube URL, reddit URL). You can also use the script
`formatmatches` to render the data in different formats (specified by the first
argument). Currently, it is capable of producing a pretty-printed version
(`pretty`) or an HTML table (`html`). If `onmatch` is undefined, the
pretty-printed version will be written to stdout.

You could, for example, send yourself an email with the results:

	onmatch="
		/.formatmatches html | mail \
			-s 'Movies found' \
			-a 'Content-Type: text/html' \
			you@yourdomain.com"

`onnomatch` defines what to do in case no matches are found. If undefined,
nothing is done.

`onerror` defines what to do when the program encounters an error. The error
message is written to the logfile and also made available in a file called
`error`. If undefined, the program will exit with status code 1.

## Exit codes

0: no issues; 1: generic error; 2: configuration error.
