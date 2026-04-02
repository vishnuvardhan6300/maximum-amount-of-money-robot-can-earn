# maximum-amount-of-money-robot-can-earn
leetcode problem No:3418

class Solution {
    public int maximumAmount(int[][] coins) {
        int m = coins.length;
        int n = coins[0].length;

        // dp[i][j][k] = max coins at (i,j) using k neutralizations
        int[][][] dp = new int[m][n][3];

        // Initialize with very small values
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < 3; k++) {
                    dp[i][j][k] = Integer.MIN_VALUE;
                }
            }
        }

        // Base case (0,0)
        if (coins[0][0] >= 0) {
            dp[0][0][0] = coins[0][0];
        } else {
            dp[0][0][0] = coins[0][0]; // take loss
            dp[0][0][1] = 0;           // neutralize
        }

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < 3; k++) {
                    if (i == 0 && j == 0) continue;

                    int val = coins[i][j];

                    int best = Integer.MIN_VALUE;

                    // from top
                    if (i > 0) {
                        best = Math.max(best, dp[i - 1][j][k]);
                    }

                    // from left
                    if (j > 0) {
                        best = Math.max(best, dp[i][j - 1][k]);
                    }

                    if (best != Integer.MIN_VALUE) {
                        // take normally
                        dp[i][j][k] = Math.max(dp[i][j][k], best + val);
                    }

                    // if negative and we can neutralize
                    if (val < 0 && k > 0) {
                        int bestPrev = Integer.MIN_VALUE;

                        if (i > 0) {
                            bestPrev = Math.max(bestPrev, dp[i - 1][j][k - 1]);
                        }
                        if (j > 0) {
                            bestPrev = Math.max(bestPrev, dp[i][j - 1][k - 1]);
                        }

                        if (bestPrev != Integer.MIN_VALUE) {
                            dp[i][j][k] = Math.max(dp[i][j][k], bestPrev);
                        }
                    }
                }
            }
        }

        return Math.max(dp[m - 1][n - 1][0],
               Math.max(dp[m - 1][n - 1][1], dp[m - 1][n - 1][2]));
    }
}