# check_memory

Reads from `/proc/meminfo` and parses it. All values present in proc meminfo can be checked against.
Labels for Values are normalized with ``strtolower`` and all non-alphabet characters are replaced by underscores, 
so ``Inactive(file)`` becomes ``inactive_file``. 

``check_memory -t TYPE -w WARN -c CRIT [-u UNIT] [-i]``

- ``-t TYPE``: possible values are parsed from ``/proc/meminfo`` 
- ``-w WARN``: if value is higher or same, warning state
- ``-c CRIT``: if value is higher or same, critical state
- ``-u UNIT``: 0-3, exponent of 1024 to divide values by, default is 0, returning kB values, 1 will return MiB values, 2 will return GiB values, 3 will return TiB values
- ``-i``: inverts the check, without this switch the measured value must not be bigger than warn and crit, with -i it must be at least as big as warn and crit

#### Examples:

check_memory -t committed_as -w 6 -c 12 -u 2
	critical if commited memory is at least 12 GiB, warns when it reaches 6 GiB

check_memory -t memfree -w 512 -c 128 -u 1 -i
        critical if less than 128 MiB memory is still free, warns if less than 512 MiB memory is still free

#### Requirements

- needs php 5 or above
- must be able to read ``/proc/meminfo``

#### Caveats

- usually almost all values of meminfo are in kB but some aren't
- make sure to check if the returned values make sense
- plugin only returns text and no performance data

#### Typical Values for TYPE

May differ on other systems.

  - memtotal 
  - memfree
  - buffers
  - cached
  - swapcached
  - active
  - inactive
  - active_anon
  - inactive_anon
  - active_file
  - inactive_file
  - unevictable, mlocked
  - swaptotal
  - swapfree
  - dirty
  - writeback
  - anonpages
  - mapped
  - shmem
  - slab
  - sreclaimable
  - sunreclaim
  - kernelstack
  - pagetables
  - nfs_unstable
  - bounce
  - writebacktmp
  - commitlimit
  - committed_as
  - vmalloctotal
  - vmallocused
  - vmallocchunk
  - hardwarecorrupted
  - anonhugepages
  - hugepages_total
  - hugepages_free
  - hugepages_rsvd
  - hugepages_surp
  - hugepagesize
  - directmap_k
  - directmap_m