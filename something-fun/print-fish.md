使用之前在数字电路里学到的状态机转移的解法
使用三个线程，一个打印<，一个打印>，一个打印_
设置好条件变量即可

```
#include <stdio.h>
#include <pthread.h>

// 1. 定义互斥锁和条件变量
pthread_mutex_t lk = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t cv  = PTHREAD_COND_INITIALIZER;

// 2. 定义共享状态
int state = 0; 

// 线程 T_a：死循环打印 <
void *T_a(void *arg) {
    while (1) {
        pthread_mutex_lock(&lk);
        // 万能模板：while (!sync_cond()) wait();
        // A 可以打印的条件：state == 0 或 state == 2 或 state == 4
        while (!(state == 0 || state == 2 || state == 4)) {
            pthread_cond_wait(&cv, &lk);
        }
        
        // 执行打印
        putchar('<');
        fflush(stdout);
        
        // 状态转移
        if (state == 0)      state = 1; // 走 <><_ 路线
        else if (state == 2) state = 3; 
        else if (state == 4) state = 5;

        // 万能模板：广播并释放锁
        pthread_cond_broadcast(&cv);
        pthread_mutex_unlock(&lk);
    }
    return NULL;
}

// 线程 T_b：死循环打印 >
void *T_b(void *arg) {
    while (1) {
        pthread_mutex_lock(&lk);
        // B 可以打印的条件：state == 0 或 state == 1 或 state == 5
        while (!(state == 0 || state == 1 || state == 5)) {
            pthread_cond_wait(&cv, &lk);
        }
        
        putchar('>');
        fflush(stdout);
        
        if (state == 0)      state = 4; // 走 ><>_ 路线
        else if (state == 1) state = 2;
        else if (state == 5) state = 6;

        pthread_cond_broadcast(&cv);
        pthread_mutex_unlock(&lk);
    }
    return NULL;
}

// 线程 T_c：死循环打印 _
void *T_c(void *arg) {
    while (1) {
        pthread_mutex_lock(&lk);
        // C 可以打印的条件：一串序列完成了（state == 3 或 state == 6）
        while (!(state == 3 || state == 6)) {
            pthread_cond_wait(&cv, &lk);
        }
        
        putchar('_');
        fflush(stdout);
        
        // 回到初始状态，开始新的一轮
        state = 0;

        pthread_cond_broadcast(&cv);
        pthread_mutex_unlock(&lk);
    }
    return NULL;
}

int main() {
    pthread_t ta, tb, tc;
    // 可以创建多个 T_a, T_b, T_c 线程，万能模板依然能保证正确性
    pthread_create(&ta, NULL, T_a, NULL);
    pthread_create(&tb, NULL, T_b, NULL);
    pthread_create(&tc, NULL, T_c, NULL);

    pthread_join(ta, NULL);
    pthread_join(tb, NULL);
    pthread_join(tc, NULL);
    return 0;
}
```
