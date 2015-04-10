# Constants #

These are the constants that can be passed as the `type` parameter to the `mf_midi_evt()` and `mf_seq_evt()` functions:

```
#define st_note_off                0x80
#define st_note_on                 0x90
#define st_key_pressure            0xA0
#define st_control_change          0xB0
#define st_program_change          0xC0
#define st_channel_pressure        0xD0
#define st_pitch_bend              0xE0
#define st_system_exclusive        0xF0
#define st_system_continue         0xF7
#define st_meta_event              0xFF
```


These are the constants that can be passed as the `aux` parameter to the `mf_sys_evt()` and to the `mf_seq_sys()` functions when type is `st_meta_event`:
```
#define me_sequence_number         0x00

#define me_text                    0x01
#define me_copyright_notice        0x02
#define me_sequence_name           0x03
#define me_track_name              0x03
#define me_instrument_name         0x04
#define me_lyric                   0x05
#define me_marker                  0x06
#define me_cue_point               0x07
#define me_device_name             0x08
#define me_program_name            0x09

#define me_end_of_track            0x2f
#define me_set_tempo               0x51
#define me_smpte_offset            0x54
#define me_time_signature          0x58
#define me_key_signature           0x59
#define me_sequencer_specific      0x7F
#define me_channel_prefix          0x20
#define me_port_prefix             0x21

```