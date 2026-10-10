gearman-top
===========

"top" for gearmand - watch queues and workers in real-time, and see at a
glance when a queue is actually in trouble.  A friendlier gearman_top /
``gearadmin --status`` in the style of htop and glances.

Single Perl script, core modules only.  Speaks the gearmand admin protocol
(``status``, ``version``) on port 4730.

USAGE
-----

::

   ./gearman-top                                  # localhost:4730, refresh 2s
   ./gearman-top --servers gm1,gm2:4731 --sleep 5
   ./gearman-top --filter '^Campaign'
   ./gearman-top --oneoff                         # exit 0 OK, 1 WARNING, 2 CRITICAL

   - Specify servers w/ --servers (multiple times or comma separated)
   - Specify default port w/ --port (defaults to 4730)
   - Specify sleep time w/ --sleep (default 2), or as a bare number
   - Specify color output w/ --color (default) or --nocolor
   - Show only functions matching a regex w/ --filter
   - Run a single time without clearing the screen: --oneoff

WHAT IT FLAGS
-------------

   - CRIT: jobs waiting on a function with no workers
   - CRIT: all workers of a function gone
   - CRIT: server unreachable
   - WARN/CRIT: all workers busy with jobs waiting (saturation), by queue
     size and how long jobs have been waiting
   - WARN/CRIT: jobs waiting while workers sit idle (stuck workers)
   - WARN: lost workers compared to the peak seen this session
   - WARN/CRIT: slow 'status' round-trip

   Rows are sorted worst first, then by queue size, and the table is cut to
   the terminal height so problems never scroll off screen.  Idle functions
   are dimmed.  TREND is the change in waiting jobs per second, AGE how long
   the function has had jobs waiting nonstop.

   Thresholds live in %limit at the top of the script.
