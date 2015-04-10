# Midi Sequences #

With MIDI _sequences_, events can be specified in any order. Timing has to be specified in absolute ticks instead of deltas.

The API is:

```
mf_seq *mf_seq_new (char *fname, short division);
int mf_seq_close(mf_seq *ms);
int mf_seq_set_track(mf_seq *ms, int track);
int mf_seq_get_track(mf_seq *ms, int track);
int mf_seq_evt (mf_seq *ms, unsigned long tick, short type, short chan, short data1, short data2);
int mf_seq_sys(mf_seq *ms, unsigned long tick, short type, short aux, long len, unsigned char *data);
int mf_seq_text(mf_seq *ms, unsigned long tick, short type, char *txt);
```

Example:

```
#include "mf_seq.h"

int main(int argc, char *argv[])
{
  mf_seq *m;

  m = mf_seq_new("ss.mid", 384);
  
  if (m) {
    mf_seq_set_track(m, 2);
    mf_seq_evt (m, 0, st_note_on, 0, 60, 90);
    mf_seq_evt (m, 192, st_note_off, 0, 60, 0);
    
    mf_seq_set_track(m, 1);
    
    mf_seq_sys(m,0,st_meta_event,me_track_name,5,"Named");
    
    mf_seq_evt (m, 0, st_note_on, 1, 64, 90);
    mf_seq_evt (m, 384, st_note_off, 1, 64, 0);
    
    mf_seq_close(m);
  }
  
}
```