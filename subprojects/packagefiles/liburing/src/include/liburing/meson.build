if has__kernel_rwf_t
  __kernel_rwf_t_compat = ''
else
  __kernel_rwf_t_compat = '''typedef int __kernel_rwf_t;
'''
endif

if has__kernel_timespec
  __kernel_timespec_compat = '''#include <linux/time_types.h>

#define UAPI_LINUX_IO_URING_H_SKIP_LINUX_TIME_TYPES_H 1
'''
else
  __kernel_timespec_compat = '''#include <stdint.h>

struct __kernel_timespec {
  int64_t    tv_sec;
  long long  tv_nsec;
};

#define UAPI_LINUX_IO_URING_H_SKIP_LINUX_TIME_TYPES_H 1
'''
endif

if has_open_how
  open_how_compat = '#include <linux/openat2.h>'
else
  open_how_compat = '''#include <inttypes.h>

struct open_how {
  uint64_t flags;
  uint64_t mode;
  uint64_t resolve;
};
'''
endif

conf_data = configuration_data()
conf_data.set('__kernel_rwf_t_compat', __kernel_rwf_t_compat)
conf_data.set('__kernel_timespec_compat', __kernel_timespec_compat)
conf_data.set('open_how_compat', open_how_compat)

if not glibc_statx and has_statx
  conf_data.set('no_glibc_statx', '#include <sys/stat.h>')
else
  conf_data.set('no_glibc_statx', '')
endif

configure_file(
  input: 'compat.h.meson',
  output: 'compat.h',
  configuration: conf_data,
  install: true,
  install_dir: get_option('includedir') / 'liburing',
)

configure_file(
  output: 'io_uring_version.h',
  configuration: configuration_data(
    {
      'IO_URING_VERSION_MAJOR': major_version,
      'IO_URING_VERSION_MINOR': minor_version,
    },
  ),
  install: true,
  install_dir: get_option('includedir') / 'liburing',
)

install_headers(
  'barrier.h',
  'io_uring.h',
  subdir: 'liburing',
)
