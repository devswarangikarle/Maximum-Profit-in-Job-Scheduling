# Maximum-Profit-in-Job-Scheduling

class Solution:
    def jobScheduling(self, startTime, endTime, profit):
        jobs = sorted(zip(startTime, endTime, profit), key=lambda x: x[1])
        n = len(jobs)

        dp = [0] * n
        dp[0] = jobs[0][2]

        for i in range(1, n):
            latest_non_overlap = self.binary_search(jobs, i)
            include_profit = jobs[i][2] + (dp[latest_non_overlap] if latest_non_overlap != -1 else 0)
            dp[i] = max(dp[i - 1], include_profit)

        return dp[-1]

    def binary_search(self, jobs, current_job_index):
        start, end = 0, current_job_index - 1
        while start <= end:
            mid = (start + end) // 2
            if jobs[mid][1] <= jobs[current_job_index][0]:
                if jobs[mid + 1][1] <= jobs[current_job_index][0]:
                    start = mid + 1
                else:
                    return mid
            else:
                end = mid - 1
        return -1


solution = Solution()

startTime1 = [1,2,3,3]
endTime1 = [3,4,5,6]
profit1 = [50,10,40,70]
print(solution.jobScheduling(startTime1, endTime1, profit1))  

startTime2 = [1,2,3,4,6]
endTime2 = [3,5,10,6,9]
profit2 = [20,20,100,70,60]
print(solution.jobScheduling(startTime2, endTime2, profit2))  

startTime3 = [1,1,1]
endTime3 = [2,3,4]
profit3 = [5,6,4]
print(solution.jobScheduling(startTime3, endTime3, profit3)) 
