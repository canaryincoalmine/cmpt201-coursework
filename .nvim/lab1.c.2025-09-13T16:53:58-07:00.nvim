#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
  printf("Please enter some text: ");
  char *buffer = NULL;
  size_t size = 0;
  ssize_t num_char = getline(&buffer, &size, stdin);

  if (num_char == -1) {
    perror("getline failed");
    exit(EXIT_FAILURE);
  }

  char *pointer;
  char *output = strtok_r(buffer, " ", &pointer);

  printf("Tokens:\n");
  while (output != NULL) {
    printf("\t%s\n", output);
    output = strtok_r(NULL, " ", &pointer);
  }
  free(buffer);

  return 0;
}
