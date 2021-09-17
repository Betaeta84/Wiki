
## Scheduler

The scheduler is a thread which provides `Job` instances these can occur a number of times over a given duration. A scheduled job will skip run cycles if the job takes longer than the interval to complete. Refreshing guis and signals are processed through scheduled jobs.

The kernel scheduler is allows for jobs to be scheduled. It launches a thread in the kernel which which iterates through the different scheduled jobs, and running them when required. This thread is also used as during shutdown to terminate everything in a safe manner.

### Jobs
Jobs are either called within a context for add_job() or a module can extend the Job and call schedule() and unschedule().

## Timers
Timers are user set internal commands that execute repeatedly.

