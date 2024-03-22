# Design a Scheduler using MaxHeap

Imagine you have a list of tasks to complete during the day, and each task has a certain duration and dependencies on other tasks; some tasks have constraints such as deadlines, and/ or happen at a fixed time that can’t be changed. The goal of this scheduler is to determine the best order to complete all the tasks based on their priority values given the constraints while maximizing your productivity.

The fairest way to determine the priority value of each task is by computing it through a formula with various factors with their significance weighted differently. This formula is further explained below. Using this value, the algorithm schedules tasks with higher priority first and leaves tasks with lower priority later. Moreover, if a task depends on other tasks that haven’t been completed, it has to wait. Otherwise, it gets added to the priority queue. If two tasks have the same priority value, the algorithm sorts them by their ID (as inputted).

There are two constraints that this algorithm was built to satisfy. 
## 1. Fixed time:
If a task happens at a fixed time, it will be scheduled at that time, regardless of its priority value.
If, because of its dependencies, a task is scheduled later than its fixed time, the scheduler reduces the duration of the task to the remaining time till the original finish time. This aims to help finish the task on time, as planned, even if we start it late, and avoid delaying the long list of remaining tasks for the rest of the day.
If the remaining time till the original finish time is less than or equal to 0, the scheduler will skip this task and move on to the next one in the priory queue. For example, the museum tour starts at 11:00 AM (fixed time) with a duration of 60 minutes, which means it ends at 12:00 PM. I get there at 12:05 PM, meaning the remaining time is negative (12:00 - 12:05 = -5 minutes), thus it does not make sense to reschedule the task because there is only one available time slot for the tour and I missed it.

## 2. Deadlines:
For deadlines, if it has passed, the code prints a message stating that the task missed the deadline, marks the task as completed, and proceeds to the next task. If at the current time, there is not enough time to complete the task by the deadline, the code will calculate the new scheduled time to ensure it can be completed by the deadline. Otherwise, it starts the task as soon as possible to make the most use of the time before the deadline.
Execution: The scheduler takes the most important task from the priority queue and executes it. The algorithm then updates the time, marks the task as completed, removes its dependencies, and adds its duration to the total time. The output includes the schedule, the total time to complete the tasks, and the total utility value gained.

## How I define and compute the priority value of each task:
Factors that influence the priority value:

Task dependencies: Tasks with more dependencies will have higher priority because they depend on the completion of other tasks, which means they could also be critical milestones that allow other tasks to proceed in a series of tasks that depend on each other. Weight 0.5
For example, to get the bus to the museum (3), I have to be ready (2); and to have breakfast with my friend (4), I have to catch the bus to get there (3). Thus, task (4) is dependent on both tasks (3) and (2). 
Even if we entered task 3 as the only dependency for task 4, the algorithm would still schedule task 4 after tasks 3 and 2, as task 3 depends on task 2. However, we are calculating the priority value based on the number of dependencies that a task has, we need to input all accumulated dependencies, instead of just the one that the task is directly dependent on.

Task duration: I personally prefer to complete longer tasks first since I have more energy earlier in the day and tend to get tired after a long day. However, this factor is not necessarily as important as resolving small tasks can also help celebrate small wins; thus, its weight = 0.2

Number of people involved: The more people are involved in a task, the more prioritized it should be, because this task not only affects me but also other people. Because this impact can be on a larger scale, it should have the highest weight = 0.7

Based on these factors, I created a formula:
Priority Value = (0.5*Task Dependencies) + (0.2* Task Duration) + (0.7 * Number of People Involved)
