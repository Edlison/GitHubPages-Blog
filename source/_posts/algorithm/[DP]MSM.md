# 矩阵连乘 (Multiplying a sequence of matrices)

我们想从两个以上矩阵连乘的式子中得到最佳的次序，使得计算时乘法运算最少。

```java
public class DynamicProgramming {

	private static final int N = 5;
	
	public static void main(String[] args) {
		int[][] C = new int[N][N];
		int[][] S = new int[N][N];
		int[] r = {4, 5, 3, 6, 4, 5};
		
		for (int i = 0; i < N; i++) {// 初始化 最小乘运算数组
			for (int j = 0; j < N; j++) {
				C[i][j] = 0;
			}
		}
		
		for (int i = 0; i < N; i++) {// 初始化 分割位置
			for (int j = 0; j < N; j++) {
				S[i][j] = 0;
			}
		}
		
		for (int p = 2; p <= N; p++) {// P 为间隔
			for (int i = 0, j; i < N - p + 1; i++) {// i 为起始值
				j = i + p - 1;// j 为终止值
				C[i][j] = C[i][i] + C[i + 1][j] + r[i] * r[i + 1] * r[j + 1];// 赋 C[i][j] 的初值方便下文比较取得最小值
				S[i][j] = i;
				for (int k = i; k< j; k++) {// k 为中间分割值 
					int temp = C[i][k] + C[k + 1][j] + r[i] * r[k + 1] * r[j + 1];
					
					if (temp < C[i][j]) {// 取得每次分割后的最小值
						C[i][j] = temp;
						S[i][j] = k;
					}
				}
			}
		}
		
		for (int i = 0; i < N; i++) {
			for (int j = 0; j < N; j++) {
				System.out.print(C[i][j] + "\t");
			}
			System.out.println();
		}
		
		System.out.println();
		
		for (int i = 0; i < N; i++) {
			for (int j = 0; j < N; j++) {
				if (j > i) System.out.print(S[i][j] + 1 + "\t");
				else System.out.print(0 + "\t");
			}
			System.out.println();
		}
	}
}

/* result
0	60	132	180	252	
0	0	90	132	207	
0	0	0	72	132	
0	0	0	0	120	
0	0	0	0	0	

0	1	2	2	2	
0	0	2	2	2	
0	0	0	3	4	
0	0	0	0	4	
0	0	0	0	0

(A1 * A2) * ((A3 * A4) * A5)
*/
```