# Writing Midi files #

To create a midifile, the following functions are available:

```
mf_writer *mf_new (char *fname, short division);
int mf_close (mf_writer *mw);

int mf_track_start (mf_writer *mw);
int mf_track_end   (mf_writer *mw);

int mf_midi_evt (mf_writer *mw, unsigned long delta, short type, short chan, short data1, short data2);
int mf_sys_evt  (mf_writer *mw, unsigned long delta, short type, short aux, long len, unsigned char *data);
                                                  
int mf_text_evt (mf_writer *mw, unsigned long delta, short type, char *txt);

```

**Example**

```
int createmidifile(char *fname)
{
   mf_writer *m;
   int ret = 999;
   
   if ((m = mf_new(fname,192))) {  ret = 0; }
   
   if (!ret)  {ret = mf_track_start(m);}
   if (!ret)  {ret = mf_midi_evt(m,  0,st_note_on ,0,64,100);}
   if (!ret)  {ret = mf_midi_evt(m,192,st_note_off,0,64,0);}

   if (!ret)  {ret = mf_midi_evt(m,  0,st_note_on ,0,67,100);}
   if (!ret)  {ret = mf_midi_evt(m,192,st_note_off,0,67,0);}
   if (!ret)  {ret = mf_track_end(m);}
   
   if (!ret)  {ret = mf_track_start(m);}
   if (!ret)  {ret = mf_midi_evt(m,  0,st_note_on ,0,60,100);}
   if (!ret)  {ret = mf_midi_evt(m,384,st_note_off,0,60,0);}
   if (!ret)  {ret = mf_track_end(m);}

   if (!ret)  {ret = mf_close(m);}
   
   if (ret)   { fprintf(stderr, "ERROR: %d\n",ret); }
   return ret;
}
```