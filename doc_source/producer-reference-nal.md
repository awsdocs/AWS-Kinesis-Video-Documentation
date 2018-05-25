# Network Abstraction Layer \(NAL\) Adaptation Flag Reference<a name="producer-reference-nal"></a>

This section contains information about available flags for the `StreamInfo.NalAdaptationFlags` enumeration\.

The [elementary stream](https://en.wikipedia.org/wiki/Elementary_stream) in an application can be in either **Annex\-B** or **AVCC** format: 
+ The **Annex\-B** format delimits [NALUs \(Network Abstraction Layer units\)](https://en.wikipedia.org/wiki/Network_Abstraction_Layer#NAL_units) with two bytes of zeroes, followed by one or three bytes of zeroes, followed by the number *1* \(called a **start code**, for example, 00000001\)\. 
+ The **AVCC** format also wraps NALUs, but each NALU is preceded by a value that indicates the size of the NALU \(usually four bytes\)\.

Many encoders produce the Annex\-B bitstream format\. Some higher\-level bitstream processors \(such as a playback engine or the [Media Source Extensions \(MSE\)](https://en.wikipedia.org/wiki/Media_Source_Extensions) player in the AWS Management Console\) use the AVCC format for their frames\.

The codec private data \(CPD\), which is SPS/PPS \(Sequence Parameter Set/Picture Parameter Set\) for the H\.264 codec, can also be in Annex\-B or AVCC format\. However, for the CPD, the formats are different from those described previously\.

The flags tell the SDK to adapt the NALUs to AVCC or Annex\-B for frame data and CPD as follows: 


****  

| Flag | Adaptation | 
| --- | --- | 
| NAL\_ADAPTATION\_FLAG\_NONE | No adaptation | 
| NAL\_ADAPTATION\_ANNEXB\_NALS | Adapt Annex\-B NALUs to AVCC NALUs | 
| NAL\_ADAPTATION\_AVCC\_NALS | Adapt AVCC NALUs to Annex\-B NALUs | 
| NAL\_ADAPTATION\_ANNEXB\_CPD\_NALS | Adapt Annex\-B NALUs for the codec private data to AVCC format NALUs | 
| NAL\_ADAPTATION\_ANNEXB\_CPD\_AND\_FRAME\_NALS | Adapt Annex\-B NALUs for the codec and frame private data to AVCC format NALUs | 

For more information about NALU types, see **Section 1\.3: Network Abstraction Layer Unit Types** in [RFC 3984](https://www.ietf.org/rfc/rfc3984.txt)\.