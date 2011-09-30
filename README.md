Adaptive streaming is a popular and rather elegant technology to adapt the
audio-video streams provided over the internet dynamically to the users' needs.
During research at VRT-medialab, we developed some tools that were useful in
creating, testing and debugging these Adaptive Bitrate streaming files. Since
these tools might be useful for other people to use, we decided to open-source
them.

This project contains:

 * A few parser scripts to dump binary formats into a "human" readable format.
   It's by no means an easy read, but has saved us many hours of watching
   hexdumps.

    * adts-parser.pl will spit out the ADTS-frames and tell you what
      type of AAC block is inside.

    * h264-parser.pl reads in raw (AnnexB) h.264 streams and figures out the
      NAL type (I-frame, non-I-frame, ...) of the packet

    * mp4-parser.pl reads in an MP4 container (MPEG 4 Part 14) and prints out
      the box structure

    * ts-tools.pl parses MPEG 2 Transport Streams. It can spit out the decoded
      text, but can also extract (demux) a single video or audio-stream out of
      the TS-file.

 * A segmenting tool. This tool splits the input stream into multiple output
   files, according to Apple's version of the adaptive streaming protocol (IETF
   draft http://tools.ietf.org/html/draft-pantos-http-live-streaming-06).
   The segmenter code compiles into 4 binaries, each using a different
   splitting algorithm: ByteCount, MP3, ADTS (AAC) and MpegtsH264. Apart from
   ByteCount, they all try to split every N seconds, but keeping the file
   structure in mind: i.e. ADTS will cut on frame boundaries, MpegtsH264 will
   cut on GOP boundaries.
   It also provides support for Live-stream-mode and supports encryption