### 内核驱动相关
#### 一.关键头文件及内含的结构体，函数：
* linux/types.h:<br>
        1.dev_t;
          int MAJOR(dev_t dev);
          int MINOR(dev_t dev);
          dev MKDEV(unsigned int major,unsigned int minor);

* linux/fs.h:<br>
        1.int register_chrdev_region(dev_t first,unsigned int count,char* name);
      2.int alloc_chrdev_region(dev_t* dev,unsigned int firstminor,unsigned int count,char* name);
      3.void unregister_chrdev_region(dev_t first,unsigned int count);
      4.struct file;
      5.struct file_operations;
      6.struct inode;

* linux/cdev.h:<br>
        1.struct cdev cdev_alloc(void);
      2.void cdev_init(struct cdev* dev,struct file_operations* fops);
      3.int cdev_add(struct cdev* dev,dev_t num,unsigned int count);
      4.void cdev_del(struct cdev* dev);

* linux/kernel.h:<br>
        1. container_of(pointer,type,field);

* asm/uaccess.h:<br>
        1. unsigned long copy_from_user(void* to,void* from,unsigned long count);
      2. unsigned long copy_to_user(void* to,void* from,unsigned long count);

* asm/semaphore.h:<br>
        1. DECLARE_MUTEX(name);
      2. DECLARE_MUTEX_LOCKED(name);
      3. init_MUTEX(struct semaphore* sem);
      4. init_MUTEX_LOCKED(struct semaphore* sem);
      5. void down(struct semaphore* sem);
      6. int down_interruptible(struct semaphore* sem);
      7. int down_trylock(struct semaphore* sem);
      8. void up(struct semaphore* sem);
      9. struct rw_semaphore;
      10. init_rwsemaphore(struct rw_semaphore* sem);
      11. void down_read(struct rw_semaphore* sem);
      12. int down_read_trylock(struct rw_semaphore* sem);
      13. void up_read(struct rw_semaphore* sem);
      14. void down_write(struct rw_semaphore* sem);
      15. int down_write_trylock(struct rw_semaphore* sem);
      16. void up_write(struct rw_semaphore* sem);
      17. void downgrade_write(struct rw_semaphore* sem);

* linux/complition.h:<br>
        1. DECLARE_COMPLITION(name);
      2. init_complition(struct complition* c);
      3. INIT_COMPLITION(struct complition* c);
      4. void wait_for_complition(struct complition* c);
      5. void complition(struct complition* c);
      6. void complition_all(struct complition* c);
      7. void complite_and_exit(struct complition*c,long retval);

* linux/spinlock.h:<br>
        1. spinlock_t lock=SPIN_LOCK_UNLOCKED;
      2. spinlock_init(spinlock_t* lock);
      3. void spin_lock(spinlock_t* lock);
      4. void spin_lock_irqsave(spinlock_t* lock,unsigned long flags);
      5. void spin_lock_irq(spinlock_t* lock);
      6. void spin_lock_bh(spinlock_t* lock);
      7. int spin_trylock(spinlock_t* lock);
      8. int spin_lock_trylock_bh(spinlock_t* lock);
      9. void spin_unlock(spinlock_t* lock);
      10. void spin_unlock_irqrestore(spinlock_t* lock,unsigned long flags);
      11. void spin_unlock_irq(spinlock_t* lock);
      12. void spin_unlock_bh(spinlock_t* lock);
      13. rwlock_t lock=RW_LOCK_UNLOCKED;
      14. rwlock_init(rwlock_t* lock);
      15. void read_lock(rwlock_t* lock);
      16. void read_lock_irqsave(rwlock_t* lock,unsigned long flags);
      17. void read_lock_irq(rwlock_t* lock);
      18. void read_lock_bh(rwlock_t* lock);
      19. void read_unlock(rwlock_t* lock);
      20. void read_unlock_irqrestore(rwlock_t* lock,unsigned long flags);
      21. void read_unlock_irq(rwlock_t* lock);
      22. void read_unlock_bh(rwlock_t* lock);
      23. void write_lock(rwlock_t* lock);
      24. void write_lock_irqsave(rwlock_t* lock,unsigned long flags);
      25. void write_lock_irq(rwlock_t* lock);
      26. void write_lock_bh(rwlock_t* lock);
      27. void write_unlock(rwlock_t* lock);
      28. void write_unlock_irqrestore(rwlock_t* lock,unsigned long flags);
      29. void write_unlock_irq(rwlock_t* lock);
      30. void write_unlock_bh(rwlock_t* lock);

* asm/atomic.h:<br>
        1. atomic_t v=ATOMIC_INIT(value);
      2. void atomic_set(atomic_t* v,int i);
      3. int atomic_read(atomic_t* v);
      4. void atomic_add(int i,atomic_t* v);
      5. void atomic_sub(int i,atomic_t* v) ;
      6. void atomic_inc(atomic_t* v);
      7. void atomic_dec(atomic* v);
      8. int atomic_inc_and_test(atomic_t* v);
      9. int atomic_dec_and_test(atomic_t* v);
      10. int atomic_sub_and_test(atomic_t* v);
      11. int atomic_add_negative(int i,atomic_t* v);
      12. int atomic_add_return(int i,atomic_t* v);
      13. int atomic_sub_return(int i,atomic_t* v);
      14. int atomic_inc_return(atomic_t* v);
      15. int atomic_dec_return(atomic_t* v);

* asm/bitops.h:<br>
        1. void set_bit(nr,void* addr);
      2. void clear_bit(nr,void* addr);
      3. void change_bit(nr,void* addr);
      4. test_bit(nr,void* addr);
      5. int test_and_set_bit(nr,void* addr);
      6. int test_and_clear_bit(nr,void* addr);
      7. int test_and_change_bit(nr,void* addr);

* linux/seqlock.h:<br>
        1. seqlock_t lock=SEQLOCK_UNLOCKED;
      2. seqlock_init(seqlock_t* lock);
      3. unsigned int read_seqbegin(seqlock_t* lock);
      4. unsigned int read_seqbegin_irqsave(seqlock_t* lock,unsigned long flags);
      5. int read_seqretry(seqlock_t* lock,unsigned int seq);
      6. int read_seqretry_irqrestore(seqlock_t* lock,unsigned int seq,unsigned long flags);
      7. void write_seqlock(seqlock_t* lock);
      8. void write_seqlock_irqsave(seqlock_t* lock,unsigned long flags);
      9. void write_seqlock_irq(seqlock_t* lock);
      10. void write_seqlock_bh(seqlock_t* lock);

* linux/rcupdate.h:<br>
        1. void rcu_read_lock;
      2. void rcu_read_unlock;
      3. void call_rcu(struct rcu_head* head,void (*func)(void* arg),void* arg);

* linux/param.h:<br>
        1. HZ;

* linux/jiffies.h:<br>
        1. volatile unsigned long jiffies;
      2. u64 jiffies_64;
      3. int time_after(unsigned long a,unsigned long b);
      4. int time_before(unsigned long a,unsigned long b);
      5. int time_after_eq(unsigned long a,unsigned long b);
      6. int time_before_eq(unsigned long a,unsigned long b);
      7. u64 get_jiffies_64(void);

* linux/time.h:<br>
        1. unsigned long timespec_to_jiffies(struct timespec* value);
      2. void jiffies_to_timespec(unsigned long jiffies,struct timespec* value);
      3. unsigned long timeval_to_jiffies(struct timeval* value);
      4. void jiffies_to_timeval(unsigned long jiffies,struct timeval* value);

* asm/msr.h:<br>
        1. rdtsc(low32,high32);
      2. rdtscl(low32);

* linux/wait.h:<br>
        1. long wait_event_interruptible_timeout(wait_queue_head_t* q,condition,signed long timeout)//睡眠直到condition为真或超时;

* linux/sched.h:<br>
        1. signed long schedule_timeout(signed long timeout);

* linux/delay.h:<br>
        1. void ndelay(unsigned long nsecs);
      2. void udelay(unsigned long usecs);
      3. void mdelay(unsigned long msecs);
      4. void msleep(unsigned int millisecs);
      5. unsigned long msleep_interruptible(unsigned int millisecs);
      6. void ssleep(unsigned int seconds);

* asm/hardirq.h:<br>
        1. int in_interrupt(void);
      2. int in_atomic(void);

* linux/timer.h:<br>
        1. void init_timer(struct timer_list* timer);
      2. struct timer_list TIMER_INITIALIZER( function,expires,data);
      3. void add_timer(struct timer_list* timer);
      4. int mod_timer(struct timer_list* timer,unsigned long expires);
      5. int timer_pending(struct timer_list* timer);
      6. void del_timer(struct timer_list* timer);
      7. void del_timer_sync(struct timer_list* timer);

* linux/interrupt.h:<br>
        1. DECLARE_TASKLET(name,func,data);
      2. DECLARE_TASKLET_DISABLE(name,func,data);

* linux/workqueue.h:<br>
        1. struct workqueue_struct;
      2. struct work_struct;
      3. struct workqueue_struct* create_workqueue(const char* name);
      4. struct workqueue_struct* create_singlethread_workqueue(const char* name);
      5. void destroy_workqueue(struct workqueue_struct* queue);
      6. DECLARE_WORK(name, void (*function)(void *), void *data);
      7. INIT_WORK(struct work_struct *work, void (*function)(void *), void *data);
      8. PREPARE_WORK(struct work_struct *work, void (*function)(void *), void *data);
      9. int queue_work(struct workqueue_struct* queue,struct work_struct* work);
      10. int queue_delayed_work(struct workqueue_struct* queue,struct work_struct* work,unsigned long delay);
      11. int cancel_delayed_work(struct work_struct* work);
      12. void flush_workqueue(struct workqueue_struct* queue);
      13. int schedule_work(struct work_struct* work);
      14. int schedule_delayed_work(struct work_struct* work,unsigned long delay);
      15. void flush_scheduled_work(void);

* linux/slab.h:<br>
        1. void* kmalloc(size_t size,int flags);
      2. void kfree(void* kobj);

* linux/mm.h:<br>
        1. GFP_USER;
      2. GFP_KERNEL;
      3. GFP_NOFS;
      4. GFP_NOIO;
      5. GFP_ATOMIC;

* linux/malloc.h:<br>
        1. kmem_cache_t* kmem_cache_create(char* name,size_t size,size_t offset,unsigned long flags,constructor(),destructor());
      2. int kmem_cache_destory(kmem_cache_t* cache);
      3. void* kmem_cache_alloc(kmem_cache_t* cache,int flags);
      4. void kmem_cache_free(kmem_cache_t* cache,const void* obj);

* linux/mempool.h:<br>
        1. mempool_t* mempool_create(int min_nr,mempool_alloc_t* alloc_fn,mempool_free_t* free_fn,void* data);
      2. void mempool_destory(mempool_t* pool);
      3. void mempool_alloc(mempool_t* pool,int gfp_mask);
      4. void mempool_free(void* element,mempool_t* pool);
      5. unsigned long get_zeroed_page(int flags);
      6. unsigned long __get_free_page(int flags);
      7. unsigned long __get_free_pages(int flags,unsigned long order);
      8. int get_order(unsigned long size);
      9. void free_page(unsigned long addr);
      10. void free_pages(unsigned long addr,unsigned long order);
      11. struct page* alloc_pages_node(int nid,unsigned int flags,unsigned int order);
      12. struct page* alloc_pages(unsigned int flags,unsigned int order);
      13. struct page* alloc_page(unsigned int flags);
      14. void __free_page(struct page* page);
      15. void __free_pages(struct page* page,unsigned int order);
      16. void free_hot_page(struct page* page);

* linux/vmalloc.h:<br>
        1. void* vmalloc(unsigned long size);
      2. void vfree(void* addr);

* asm/io.h:<br>
        1. void* ioremap(unsigned long offset,unsigned long size);
      2. void iounmap(void* addr);

* linux/percpu.h:<br>
        1. DEFINE_PER_CPU(type,name);
      2. DECLARE_PER_CPU(type,name);
      3. per_cpu(variable,int cpu_id);
      4. get_cpu_var(variable);
      5. put_cpu_var(variable);
      6. void* alloc_percpu(type);
      7. void* __alloc_percpu(size_t size,size_t align);
      8. free_percpu(void* variable);
      9. int get_cpu();
      10. void put_cpu();
      11. per_cpu_ptr(void* variable,int cpu_id);

* linux/bootmem.h:<br>
        1. void* alloc_bootmem(unsigned long size);
      2. void* alloc_bootmem_low(unsigned long size);
      3. void* alloc_bootmem_pages(unsigned long size);
      4. void* alloc_bootmem_low_pages(unsigned long size);
      5. void free_bootmem(unsigned long addr,unsigned long size);

* linux/interrupt.h:<br>
        1.int request_irq(unsigned int irq, irqreturn_t (*handler)( ), unsigned long flags, constchar *dev_name, void *dev_id);
      2.void free_irq(unsigned int irq, void *dev_id);

* linux/irq.h:<br>
        1. int can_request_irq(unsigned int irq, unsigned long flags);

* asm/signal.h:<br>
        1. unsigned long probe_irq_on(void);
      2. int probe_irq_off(unsigned long);
      3. void disable_irq(int irq);
      4. void disable_irq_nosync(int irq);
      5. void enable_irq(int irq);
      6. void local_irq_save(unsigned long flags);
      7. void local_irq_restore(unsigned long flags);
      8. void local_irq_disable(void);
      9. void local_irq_enable(void);
      
