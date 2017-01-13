# aioscheduler

a Python scheduler library

The library is intended to provide a synchronized, distributed scheduler for automating tasks based on asyncio.

## Features

* Support for synchronization of schedulers on different machines
* In memory jobstore
* Persistent redis jobstore relying on aioredis
* Schedule one time jobs to be executed now or at a specific time
* Schedule repeating jobs with optional start and finish times
* Schedule jobs to be executed on a specific keyword

## Usage

The easiest way to use aioscheduler in your own project is to import it as a git submodule.

Navigate to the directory where you would like to add the aioscheduler submodule.

Then, type 

```shell
$ git submodule add https://github.com/Mishki/aioscheduler
```

For more information on working on a project with submodules, check out [git's documentation][gitdoc].

## Disclaimer

This scheduler is a work in progress and has lots of known issues. Some of the things that we aim to implement / or acknowledge are less than optimal are:

* Dynamic executors - instead of loading executors using the ```eid``` naming convention, forcing files to be in specific files, we want to make the interface much more dynamic, so that you could pass class instances, rather than needing to write code in a separate file
* The current job design is restricting and poorly defined - we want to make it better
* The ```alert``` function is misnamed and could be better implemented
* the internal ```_timer``` coroutine that puts job tasks on the event loop is restricting and should be more generic
* The current design decision of storing results as a dict of time stamp: result key: value pairs is not ideal
* There is potential for better ```add_done_callback()``` functionality
* There should be some decision about whether a ```jid``` or the entire ```job``` should be the user's handle to referring to a specific job within the scheduler.

Furthermore, the following features need more testing:

* The sync method of the redis jobstore (using ```WATCH``` and a semaphore to ensure that only one machine gets a job)
* Synchronizing clocks on different machines
* ​

## Requirements

* Python 3.3 and asyncio or Python 3.4+
* aioredis

## Contribute

* Issue Tracker: https://github.com/Mishki/aioscheduler/issues
* Source Code: https://github.com/Mishki/aioscheduler
* Other: angela.fox@findmine.com

Please don't hesitate to reach out, file an issue, or make a pull request! We would love to hear from you and collaborate with you.

## License

aioscheduler is offered under the MIT License

## Contents

### Getting Started

PUT STUFF HERE

### Basic Concepts

aioscheduler has the following components:

* Scheduler
* Jobstore
* Job
* Executor
* Alert (should not be called alert)

The *Scheduler* serves as the main interface for scheduling, modifying, stopping or removing a job from the jobstore. It also keeps track of which jobs are running, and the relevant coroutine futures associated with jobs that are about to be run. 

The *Jobstore* houses the jobs that have been scheduled and their associated results. The default jobstore is in memory, but there is also a persistent redis jobstore that schedulers on different machines can optimistically compete for jobs. Furthermore, additional custom jobstores can be created by inheriting from the base jobstore, and overriding the necessary methods.

A *Job* contains the necessary parameters that define when a job should be run. Jobs require an ```eid``` or executor id which specifies what executor should be fired when a job is run. A job can be a 'onetime' job, a repeating job, or a keyword job. Keyword jobs are run when the scheduler's ```fire_job(kewyord)``` is called. Every job also has a unique ```jid``` or job id.

An *Executor* is the actual code that is executed when a job is run. Executors must have a coroutine ```run``` defined. Currently, the naming convention is that you name your executor ```executor#.py``` where ```#``` is the integer ```eid``` that you gave to the job. Furthermore, you have to put your custom executor in the directory called ```executors```.

The *alert* function is a coroutine that must be provided to the scheduler in order to ensure coordination between the machines with schedulers. 

### Synchronization Flow

PUT SOME DIAGRAMS AND STUFF HERE

- explain sync here 
- explain alert and handle alert here

### Scheduler Internals

EXPLAIN STUFF HERE

- explain _timer here


- explain fire_job here too

### When a Job is Running

* talk about running and futures

### Upon Completion of a job

Talk about 

* the fact that a job gets removed once it's done

- add_result()
- job_complete()
- callbacks?

#### Creating a Jobstore

TODO

#### Creating a Scheduler

When creating a scheduler, you will need to provide a jobstore instance and an alert function. After creating a scheduler, you will want to start it. Calling ```start()``` simply retrieves all the jobs from the specified jobstore. If the jobstore is in memory, jobs will not be persistent. 

#### Creating a Job

A Job should be initialized with a dictionary ```params``` that must contain a key value pair:

> * 'eid' : the executor id that indicates what executor should be run

The ```params``` argument could also the following key value pairs:

> * 'when' : a datetime object that indicates when the job should be run for the first time
> * 'kwd' : a keyword string that can be associated with a job, so 

#### Starting a Scheduler

#### Scheduling a Job

When you want to schedule a job, you do so by calling ```schedule(job)``` and passing the specific job to the scheduler. Internally, the scheduler does three things:

1. the scheduler uses its jobstore's ```add_job(job)``` coroutine to add the job to the jobstore
2. the scheduler calls the ```alert(jid)``` coroutine, to notify the other machines that a job was added.
3. the scheduler calls the ```set_timer(jid)``` method to set the timer that will count down till the correct second, and then run the job and fire the job's executor.

#### Modifying a Job

Modifying a job is simple using the ```modify(job)``` coroutine. Internally, the scheduler by calling its jobstore's ```modify(job)``` coroutine, then it calles the ```alert(jid)``` coroutine, to notify the other machines to reset their timer for that specific job.  Then, it updates its own timer for the modified job.

#### Stopping a Job or Jobs

The scheduler provides an interface for stopping a specific job as well as stopping all jobs. You can stop a job using the ```stop_job(job)``` coroutine, and you can stop all jobs with the ```stop_jobs(job)``` coroutine. 

First, the scheduler alerts all the other machines that a job or jobs need to be stopped. Then, each machine's scheduler checks if it is the one running the job, and if it is, it cancels that task's future, stopping the job. The job remains in the jobstore, so it can be re-started if desired

**Note: If you stop a job before it has completed, it will be canceled at whatever point it was in its execution. At the moment, if it had already written a result to the results store, that result will not be erased. **

#### Starting a Job

It's easy to start a job using the scheduler's ```start_job(job)``` interface. First, the scheduler checks that the job actually exists in the job store, and the scheduler also checks that this job is not currently running on that specific machine. 

*There is a sync problem in starting a job at the moment - the other machines are currently not checking that the job is not running on their machines.*

#### Removing a Job or Jobs

Removing a job or multiple jobs is also possible. You can use the scheduler's ```remove_job(job)``` interface, or ```remove_jobs()``` to remove all jobs. First the scheduler will stop the job, if it is running on that machine, then it will remove the job from the jobstore, then it will alert the other machines.

**Note: when a job has completed, it will be automatically removed from the jobstore, but it's results will not be removed.**

#### Determining if a Job exists

You can determine if a job exists by using the ```job_exists(jid)``` coroutine. Since jobs are removed when they have completed, it can also be used to see if a known job has been completed.

#### Getting a job



#### Getting the result from a job

TODO

#### Shutting down a Scheduler

TODO



[gitdoc]: https://git-scm.com/book/en/v2/Git-Tools-Submodules













