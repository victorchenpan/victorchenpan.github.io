### 内核驱动相关<br>

### 一.关键头文件及内含的结构体，函数：<br>


* linux/types.h:<br>
> 1.dev_t;<br>
  2.int MAJOR(dev_t dev);<br>
  3.int MINOR(dev_t dev);<br>
  4.dev MKDEV(unsigned int major,unsigned int minor);<br>

* linux/fs.h:<br>
> 1.int register_chrdev_region(dev_t first,unsigned int count,char* name);<br>
  2.int alloc_chrdev_region(dev_t* dev,unsigned int firstminor,unsigned int count,char* name);<br>
  3.void unregister_chrdev_region(dev_t first,unsigned int count);<br>
  4.struct file;<br>
  5.struct file_operations;<br>
  6.struct inode;<br>

* linux/cdev.h:<br>
> 1.struct cdev cdev_alloc(void);<br>
  2.void cdev_init(struct cdev* dev,struct file_operations* fops);<br>
  3.int cdev_add(struct cdev* dev,dev_t num,unsigned int count);<br>
  4.void cdev_del(struct cdev* dev);<br>

* linux/kernel.h:<br>
> 1.container_of(pointer,type,field);<br>

* asm/uaccess.h:<br>
> 1. unsigned long copy_from_user(void* to,void* from,unsigned long count);<br>
  2. unsigned long copy_to_user(void* to,void* from,unsigned long count);

* asm/semaphore.h:<br>
> 1. DECLARE_MUTEX(name);<br>
  2. DECLARE_MUTEX_LOCKED(name);<br>
  3. init_MUTEX(struct semaphore* sem);<br>
  4. init_MUTEX_LOCKED(struct semaphore* sem);<br>
  5. void down(struct semaphore* sem);<br>
  6. int down_interruptible(struct semaphore* sem);<br>
  7. int down_trylock(struct semaphore* sem);<br>
  8. void up(struct semaphore* sem);<br>
  9. struct rw_semaphore;<br>
  10. init_rwsemaphore(struct rw_semaphore* sem);<br>
  11. void down_read(struct rw_semaphore* sem);<br>
  12. int down_read_trylock(struct rw_semaphore* sem);<br>
  13. void up_read(struct rw_semaphore* sem);<br>
  14. void down_write(struct rw_semaphore* sem);<br>
  15. int down_write_trylock(struct rw_semaphore* sem);<br>
  16. void up_write(struct rw_semaphore* sem);<br>
  17. void downgrade_write(struct rw_semaphore* sem);<br>

* linux/complition.h:<br>
> 1. DECLARE_COMPLITION(name);<br>
  2. init_complition(struct complition* c);<br>
  3. INIT_COMPLITION(struct complition* c);<br>
  4. void wait_for_complition(struct complition* c);<br>
  5. void complition(struct complition* c);<br>
  6. void complition_all(struct complition* c);<br>
  7. void complite_and_exit(struct complition*c,long retval);<br>

* linux/spinlock.h:<br>
> 1. spinlock_t lock=SPIN_LOCK_UNLOCKED;<br>
  2. spinlock_init(spinlock_t* lock);<br>
  3. void spin_lock(spinlock_t* lock);<br>
  4. void spin_lock_irqsave(spinlock_t* lock,unsigned long flags);<br>
  5. void spin_lock_irq(spinlock_t* lock);<br>
  6. void spin_lock_bh(spinlock_t* lock);<br>
  7. int spin_trylock(spinlock_t* lock);<br>
  8. int spin_lock_trylock_bh(spinlock_t* lock);<br>
  9. void spin_unlock(spinlock_t* lock);<br>
  10. void spin_unlock_irqrestore(spinlock_t* lock,unsigned long flags);<br>
  11. void spin_unlock_irq(spinlock_t* lock);<br>
  12. void spin_unlock_bh(spinlock_t* lock);<br>
  13. rwlock_t lock=RW_LOCK_UNLOCKED;<br>
  14. rwlock_init(rwlock_t* lock);<br>
  15. void read_lock(rwlock_t* lock);<br>
  16. void read_lock_irqsave(rwlock_t* lock,unsigned long flags);<br>
  17. void read_lock_irq(rwlock_t* lock);<br>
  18. void read_lock_bh(rwlock_t* lock);<br>
  19. void read_unlock(rwlock_t* lock);<br>
  20. void read_unlock_irqrestore(rwlock_t* lock,unsigned long flags);<br>
  21. void read_unlock_irq(rwlock_t* lock);<br>
  22. void read_unlock_bh(rwlock_t* lock);<br>
  23. void write_lock(rwlock_t* lock);<br>
  24. void write_lock_irqsave(rwlock_t* lock,unsigned long flags);<br>
  25. void write_lock_irq(rwlock_t* lock);<br>
  26. void write_lock_bh(rwlock_t* lock);<br>
  27. void write_unlock(rwlock_t* lock);<br>
  28. void write_unlock_irqrestore(rwlock_t* lock,unsigned long flags);<br>
  29. void write_unlock_irq(rwlock_t* lock);<br>
  30. void write_unlock_bh(rwlock_t* lock);<br>

* asm/atomic.h:<br>
> 1. atomic_t v=ATOMIC_INIT(value);<br>
  2. void atomic_set(atomic_t* v,int i);<br>
  3. int atomic_read(atomic_t* v);<br>
  4. void atomic_add(int i,atomic_t* v);<br>
  5. void atomic_sub(int i,atomic_t* v) ;<br>
  6. void atomic_inc(atomic_t* v);<br>
  7. void atomic_dec(atomic* v);<br>
  8. int atomic_inc_and_test(atomic_t* v);<br>
  9. int atomic_dec_and_test(atomic_t* v);<br>
  10. int atomic_sub_and_test(atomic_t* v);<br>
  11. int atomic_add_negative(int i,atomic_t* v);<br>
  12. int atomic_add_return(int i,atomic_t* v);<br>
  13. int atomic_sub_return(int i,atomic_t* v);<br>
  14. int atomic_inc_return(atomic_t* v);<br>
  15. int atomic_dec_return(atomic_t* v);<br>

* asm/bitops.h:<br>
>  1. void set_bit(nr,void* addr);<br>
  2. void clear_bit(nr,void* addr);<br>
  3. void change_bit(nr,void* addr);<br>
  4. test_bit(nr,void* addr);<br>
  5. int test_and_set_bit(nr,void* addr);<br>
  6. int test_and_clear_bit(nr,void* addr);<br>
  7. int test_and_change_bit(nr,void* addr);<br>

* linux/seqlock.h:<br>
> 1. seqlock_t lock=SEQLOCK_UNLOCKED;<br>
  2. seqlock_init(seqlock_t* lock);<br>
  3. unsigned int read_seqbegin(seqlock_t* lock);<br>
  4. unsigned int read_seqbegin_irqsave(seqlock_t* lock,unsigned long flags);<br>
  5. int read_seqretry(seqlock_t* lock,unsigned int seq);<br>
  6. int read_seqretry_irqrestore(seqlock_t* lock,unsigned int seq,unsigned long flags);<br>
  7. void write_seqlock(seqlock_t* lock);<br>
  8. void write_seqlock_irqsave(seqlock_t* lock,unsigned long flags);<br>
  9. void write_seqlock_irq(seqlock_t* lock);<br>
  10. void write_seqlock_bh(seqlock_t* lock);<br>

* linux/rcupdate.h:<br>
> 1. void rcu_read_lock;<br>
  2. void rcu_read_unlock;<br>
  3. void call_rcu(struct rcu_head* head,void (*func)(void* arg),void* arg);<br>

* linux/param.h:<br>
>  1. HZ;<br>

* linux/jiffies.h:<br>
> 1. volatile unsigned long jiffies;<br>
  2. u64 jiffies_64;<br>
  3. int time_after(unsigned long a,unsigned long b);<br>
  4. int time_before(unsigned long a,unsigned long b);<br>
  5. int time_after_eq(unsigned long a,unsigned long b);<br>
  6. int time_before_eq(unsigned long a,unsigned long b);<br>
  7. u64 get_jiffies_64(void);<br>

* linux/time.h:<br>
> 1. unsigned long timespec_to_jiffies(struct timespec* value);<br>
  2. void jiffies_to_timespec(unsigned long jiffies,struct timespec* value);<br>
  3. unsigned long timeval_to_jiffies(struct timeval* value);<br>
  4. void jiffies_to_timeval(unsigned long jiffies,struct timeval* value);<br>

* asm/msr.h:<br>
> 1. rdtsc(low32,high32);<br>
  2. rdtscl(low32);<br>

* linux/wait.h:<br>
> 1. long wait_event_interruptible_timeout(wait_queue_head_t* q,condition,signed long timeout)//睡眠直到condition为真或超时;<br>

* linux/sched.h:<br>
> 1. signed long schedule_timeout(signed long timeout);<br>

* linux/delay.h:<br>
> 1. void ndelay(unsigned long nsecs);<br>
  2. void udelay(unsigned long usecs);<br>
  3. void mdelay(unsigned long msecs);<br>
  4. void msleep(unsigned int millisecs);<br>
  5. unsigned long msleep_interruptible(unsigned int millisecs);<br>
  6. void ssleep(unsigned int seconds);<br>

* asm/hardirq.h:<br>
> 1. int in_interrupt(void);<br>
  2. int in_atomic(void);<br>

* linux/timer.h:<br>
> 1. void init_timer(struct timer_list* timer);<br>
  2. struct timer_list TIMER_INITIALIZER( function,expires,data);<br>
  3. void add_timer(struct timer_list* timer);<br>
  4. int mod_timer(struct timer_list* timer,unsigned long expires);<br>
  5. int timer_pending(struct timer_list* timer);<br>
  6. void del_timer(struct timer_list* timer);<br>
  7. void del_timer_sync(struct timer_list* timer);<br>

* linux/interrupt.h:<br>
> 1. DECLARE_TASKLET(name,func,data);<br>
  2. DECLARE_TASKLET_DISABLE(name,func,data);<br>

* linux/workqueue.h:<br>
> 1. struct workqueue_struct;<br>
  2. struct work_struct;<br>
  3. struct workqueue_struct* create_workqueue(const char* name);<br>
  4. struct workqueue_struct* create_singlethread_workqueue(const char* name);<br>
  5. void destroy_workqueue(struct workqueue_struct* queue);<br>
  6. DECLARE_WORK(name, void (*function)(void *), void *data);<br>
  7. INIT_WORK(struct work_struct *work, void (*function)(void *), void *data);<br>
  8. PREPARE_WORK(struct work_struct *work, void (*function)(void *), void *data);<br>
  9. int queue_work(struct workqueue_struct* queue,struct work_struct* work);<br>
  10. int queue_delayed_work(struct workqueue_struct* queue,struct work_struct* work,unsigned long delay);<br>
  11. int cancel_delayed_work(struct work_struct* work);<br>
  12. void flush_workqueue(struct workqueue_struct* queue);<br>
  13. int schedule_work(struct work_struct* work);<br>
  14. int schedule_delayed_work(struct work_struct* work,unsigned long delay);<br>
  15. void flush_scheduled_work(void);<br>

* linux/slab.h:<br>
> 1. void* kmalloc(size_t size,int flags);<br>
  2. void kfree(void* kobj);<br>

* linux/mm.h:<br>
> 1. GFP_USER;<br>
  2. GFP_KERNEL;<br>
  3. GFP_NOFS;<br>
  4. GFP_NOIO;<br>
  5. GFP_ATOMIC;<br>

* linux/malloc.h:<br>
> 1. kmem_cache_t* kmem_cache_create(char* name,size_t size,size_t offset,unsigned long flags,constructor(),destructor());<br>
  2. int kmem_cache_destory(kmem_cache_t* cache);<br>
  3. void* kmem_cache_alloc(kmem_cache_t* cache,int flags);<br>
  4. void kmem_cache_free(kmem_cache_t* cache,const void* obj);<br>

* linux/mempool.h:<br>
> 1. mempool_t* mempool_create(int min_nr,mempool_alloc_t* alloc_fn,mempool_free_t* free_fn,void* data);<br>
  2. void mempool_destory(mempool_t* pool);<br>
  3. void mempool_alloc(mempool_t* pool,int gfp_mask);<br>
  4. void mempool_free(void* element,mempool_t* pool);<br>
  5. unsigned long get_zeroed_page(int flags);<br>
  6. unsigned long __get_free_page(int flags);<br>
  7. unsigned long __get_free_pages(int flags,unsigned long order);<br>
  8. int get_order(unsigned long size);<br>
  9. void free_page(unsigned long addr);<br>
  10. void free_pages(unsigned long addr,unsigned long order);<br>
  11. struct page* alloc_pages_node(int nid,unsigned int flags,unsigned int order);<br>
  12. struct page* alloc_pages(unsigned int flags,unsigned int order);<br>
  13. struct page* alloc_page(unsigned int flags);<br>
  14. void __free_page(struct page* page);<br>
  15. void __free_pages(struct page* page,unsigned int order);<br>
  16. void free_hot_page(struct page* page);<br>

* linux/vmalloc.h:<br>
> 1. void* vmalloc(unsigned long size);<br>
  2. void vfree(void* addr);<br>

* asm/io.h:<br>
> 1. void* ioremap(unsigned long offset,unsigned long size);<br>
  2. void iounmap(void* addr);<br>

* linux/percpu.h:<br>
> 1. DEFINE_PER_CPU(type,name);<br>
  2. DECLARE_PER_CPU(type,name);<br>
  3. per_cpu(variable,int cpu_id);<br>
  4. get_cpu_var(variable);<br>
  5. put_cpu_var(variable);<br>
  6. void* alloc_percpu(type);<br>
  7. void* __alloc_percpu(size_t size,size_t align);<br>
  8. free_percpu(void* variable);<br>
  9. int get_cpu();<br>
  10. void put_cpu();<br>
  11. per_cpu_ptr(void* variable,int cpu_id);<br>

* linux/bootmem.h:<br>
> 1. void* alloc_bootmem(unsigned long size);<br>
  2. void* alloc_bootmem_low(unsigned long size);<br>
  3. void* alloc_bootmem_pages(unsigned long size);<br>
  4. void* alloc_bootmem_low_pages(unsigned long size);<br>
  5. void free_bootmem(unsigned long addr,unsigned long size);<br>

* linux/interrupt.h:<br>
> 1.int request_irq(unsigned int irq, irqreturn_t (*handler)( ), unsigned long flags, constchar *dev_name, void *dev_id);<br>
  2.void free_irq(unsigned int irq, void *dev_id);<br>

* linux/irq.h:<br>
> 1. int can_request_irq(unsigned int irq, unsigned long flags);<br>

* asm/signal.h:<br>
> 1. unsigned long probe_irq_on(void);<br>
  2. int probe_irq_off(unsigned long);<br>
  3. void disable_irq(int irq);<br>
  4. void disable_irq_nosync(int irq);<br>
  5. void enable_irq(int irq);<br>
  6. void local_irq_save(unsigned long flags);<br>
  7. void local_irq_restore(unsigned long flags);<br>
  8. void local_irq_disable(void);<br>
  9. void local_irq_enable(void);<br>
