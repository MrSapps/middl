# Reading midifiles #

To read a midifile you need to define 5 handler functions and pass them to the mf\_read() function.

```

int mf_read( char          *fname      ,  // Midi file to read
             mf_fn_error    fn_error   ,  // called on error
             mf_fn_header   fn_header  ,  // called when the header is read
             mf_fn_track    fn_track   ,  // called when a track starts or ends
             mf_fn_midi_evt fn_midi_evt,  // called on each midi event
             mf_fn_sys_evt  fn_sys_evt    // called on each sys event
           )

```

The handler functions will have to have the following prototypes:
```
int fn_error    (short err, char *msg);
int fn_header   (short type, short ntracks, short division);
int fn_track    (short eot, short tracknum, unsigned long tracklen);
int fn_midi_evt (unsigned long delta, short type, short chan, short data1, short data2);
int fn_sys_evt  (unsigned long delta, short type, short aux, long len, unsigned char *data);
```

**Example** Dump a midi file

This is a simple way of creating an ASCII representation of a midi file.

```

#include "mf_rw.h"

int dmp_header (short type, short ntracks, short division)
{
  printf("HEADER: %u, %u, %u\n", type, ntracks, division);
  return 0;
}

int dmp_track (short eot, short tracknum, unsigned long tracklen)
{
  printf("TRACK %s: %d (%lu %s)\n", eot?"END":"START", tracknum, tracklen,eot?"ticks":"bytes");
  return 0;
}

int dmp_midi_evt(unsigned long tick, short type, short chan,
                                                  short data1, short data2)
{
  printf("%8ld %02X %02X %02X", tick, type, chan, data1);
  if (data2 >= 0) printf(" %02X", data2);  /* data2 < 0 means there's no data2! */
  printf("\n");
  return 0;
}

int dmp_sys_evt(unsigned long tick, short type, short aux,
                                               long len, unsigned char *data)
{
  printf("%8ld %02X ", tick, type);
  if (aux >= 0) printf("%02X ", aux);
  printf("%04lX ", (unsigned long)len);
  type = (type == 0xFF && (0x01 <= aux && aux <= 0x09));
  if (type) {  while (len-- > 0) printf("%c", *data++);   }  // ASCII
  else      {  while (len-- > 0) printf("%02X", *data++); }  // DATA
  printf("\n");

  return 0;
}

int dmp_error(short err, char *msg)
{
  if (msg == NULL) msg = "";
  fprintf(stderr, "Error %d - %s\n", err, msg);
  return err;
};


int main(int argc, char *argv[])
{
  if (argc > 1) {
    mf_read(argv[1],dmp_error, dmp_header, dmp_track, dmp_midi_evt, dmp_sys_evt);
  }

  return 0;
}

```