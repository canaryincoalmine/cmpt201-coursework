#include <errno.h>
#include <inttypes.h>
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

struct header {
  uint64_t size;
  struct header *next;
};

void print_out(char *format, void *data, size_t data_size) {
  int BUF_SIZE = 256;
  char buf[BUF_SIZE];
  ssize_t len = snprintf(buf, BUF_SIZE, format,
                         data_size == sizeof(uint64_t) ? *(uint64_t *)data
                                                       : *(void **)data);
  if (len < 0) {
    perror("snprintf");
  }
  write(STDOUT_FILENO, buf, len);
}

int main() {
  void *start_address = sbrk(256);

  struct header *block_one = (struct header *)start_address;
  block_one->size = 128;
  block_one->next = NULL;

  void *start_address_two = start_address + 128;

  struct header *block_two = (struct header *)start_address_two;
  block_two->size = 128;
  block_two->next = block_one;

  int size = 16;

  memset(start_address + size, 0, 128 - size);
  memset(start_address_two + size, 1, 128 - size);

  print_out("first block:        %p\n", block_one, sizeof(block_one));
  print_out("second block:       %p\n", block_two, sizeof(block_two));
  print_out("first block size:   %" PRIu64 "\n", &(block_one->size),
            sizeof(block_one->size));
  print_out("first block next:   %p\n", &(block_one->next),
            sizeof(block_one->next));
  print_out("second block size:  %" PRIu64 "\n", &(block_two->size),
            sizeof(block_two->size));
  print_out("second block next:  %p\n", &(block_two->next),
            sizeof(block_two->next));

  uint64_t *num_pointer_one = start_address + size;
  for (int i = 0; i < 128 - size; ++i) {
    print_out("%" PRIu64 "\n", &(num_pointer_one[i]), sizeof(uint64_t));
  }
  uint64_t *num_pointer_two = start_address_two + size;
  for (int i = 0; i < 128 - size; ++i) {
    print_out("%" PRIu64 "\n", &(num_pointer_two[i]), sizeof(uint64_t));
  }
  return 0;
}
