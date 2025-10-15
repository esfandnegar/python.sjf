# Preemptive SJF Scheduling (SRTF)
# Coded by ChatGPT - OS Class Example

def findWaitingTime(processes, n, bt, at, wt):
    rt = bt.copy()  # Remaining time for each process
    complete = 0
    t = 0
    minm = 999999999
    short = 0
    check = False

    while complete != n:
        for j in range(n):
            if (at[j] <= t) and (rt[j] < minm) and (rt[j] > 0):
                minm = rt[j]
                short = j
                check = True
        if not check:
            t += 1
            continue

        rt[short] -= 1
        minm = rt[short]
        if minm == 0:
            minm = 999999999

        if rt[short] == 0:
            complete += 1
            check = False
            fint = t + 1
            wt[short] = fint - bt[short] - at[short]
            if wt[short] < 0:
                wt[short] = 0
        t += 1


def findTurnAroundTime(processes, n, bt, wt, tat):
    for i in range(n):
        tat[i] = bt[i] + wt[i]


def findavgTime(processes, n, bt, at):
    wt = [0] * n
    tat = [0] * n

    findWaitingTime(processes, n, bt, at, wt)
    findTurnAroundTime(processes, n, bt, wt, tat)

    total_wt = sum(wt)
    total_tat = sum(tat)

    print("\n--- SJF (Preemptive) Scheduling ---")
    print("Process\tAT\tBT\tWT\tTAT")

    for i in range(n):
        print(f"P{processes[i]}\t{at[i]}\t{bt[i]}\t{wt[i]}\t{tat[i]}")

    print(f"\nAverage Waiting Time: {total_wt / n:.2f}")
    print(f"Average Turnaround Time: {total_tat / n:.2f}")


if __name__ == "__main__":
    n = int(input("Enter number of processes: "))

    processes = [i + 1 for i in range(n)]
    burst_time = []
    arrival_time = []

    for i in range(n):
        at = int(input(f"Enter Arrival Time for Process P{i + 1}: "))
        bt = int(input(f"Enter Burst Time for Process P{i + 1}: "))
        arrival_time.append(at)
        burst_time.append(bt)

    findavgTime(processes, n, burst_time, arrival_time)
