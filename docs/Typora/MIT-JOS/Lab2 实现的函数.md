# Lab2 实现的函数

## Part1

- `void * boot_alloc(uint32_t n)`

> simple physical memory allocator

- `void page_init(void)`

> Initialize page structure and memory free list

- `struct PageInfo * page_alloc(int alloc_flags)`

> Allocates a physical page

- `void page_free(struct PageInfo *pp)`

> Return a page to the free list

## Part 2

- `pte_t * pgdir_walk(pde_t *pgdir,const void *va,int create)`

> Given 'pgdir', a pointer to a page directory, 
>
> returns a pointer to the page table entry (PTE) for linear address 'va'

- `void boot_map_region(pde_t *pgdir,uintptr_t va,size_t size,physaddr_t pa,int perm)`

>  Map [va, va+size) of virtual address space to physical [pa, pa+size) in the page table rooted at pgdir

- `struct PageInfo * page_lookup(pde_t *pgdir,void *va,pte_t **pte_store )`

> Return the page mapped at virtual address 'va'. pte_store store the address of the pte for this page.

- `void page_remove(pde_t *pgdir,void *va)`

> Unmaps the physical page at virtuasl address 'va'

- `int page_insert(pde_t *pgdir,struct PageInfo *pp,void *va,int perm)`

> Map the physical page 'pp' at virtual address 'va',return 0 on sucess

### 一些用到的函数

* `physaddr_t page2pa(struct PageInfo *pp)`

> return page's physical address

* `struct PageInfo* pa2page(physaddr_t pa)` 

> return a pointer to page at physical address 'pa'

* `void* page2kva(struct PageInfo *pp)`

> return page's virtual address