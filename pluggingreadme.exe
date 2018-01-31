rawframeworkproject
squirrelsetup.log
hookatom.negg
hook(rawframeworkproject=execute)
safetyfirst.net
/* Start with the default size. */
  if (getsockopt (sfd, SOL_SOCKET, SO_SNDBUF, &old_size, &intsize)) {
    if (verbosity > 0) {
      perror ("getsockopt (SO_SNDBUF)");
    }
    return;
  }

  /* Binary-search for the real maximum. */
  min = last_good = old_size;
  max = MAX_UDP_SENDBUF_SIZE;

  while (min <= max) {
    avg = ((unsigned int) min + max) / 2;
    if (setsockopt (sfd, SOL_SOCKET, SO_SNDBUF, &avg, intsize) == 0) {
      last_good = avg;
      min = avg + 1;
    } else {
      max = avg - 1;
    }
  }

  vkprintf (2, "<%d send buffer was %d, now %d\n", sfd, old_size, last_good);
}

/*
 * Sets a socket's receive buffer size to the maximum allowed by the system.
 */
void maximize_rcvbuf (int sfd, int max) {
  socklen_t intsize = sizeof(int);
  int last_good = 0;
  int min, avg;
  int old_size;

  if (max <= 0) {
    max = MAX_UDP_RCVBUF_SIZE;
  }

  /* Start with the default size. */
  if (getsockopt (sfd, SOL_SOCKET, SO_RCVBUF, &old_size, &intsize)) {
    if (verbosity > 0) {
      perror ("getsockopt (SO_RCVBUF)");
    }
    return;
  }

  /* Binary-search for the real maximum. */
  min = last_good = old_size;
  max = MAX_UDP_RCVBUF_SIZE;

  while (min <= max) {
    avg = ((unsigned int) min + max) / 2;
    if (setsockopt (sfd, SOL_SOCKET, SO_RCVBUF, &avg, intsize) == 0) {
      last_good = avg;
      min = avg + 1;
    } else {
      max = avg - 1;
    }
  }

  vkprintf (2, ">%d receive buffer was %d, now %d\n", sfd, old_size, last_good);
}

int tcp_maximize_buffers;

int server_socket (int port, struct in_addr in_addr, int backlog, int mode) {
  int sfd;
  struct linger ling = {0, 0};
  int flags = 1;
