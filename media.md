FFmpeg学习可以参看源码下面的doc/examples

FFMEPG库：
1）avcodec：编解码（最重要的库）
2）avformat：封装格式处理
3）avfiler：滤镜特效处理
4）avdevice：各种设备的输入输出
5）avutil：工具库
6）postpro：后加工
7）swresaple：音频采样数据格式转换
8）swscale：视频像素格式转换

FFmpeg解码函数简介：
1）av_register_all():注册所有组件
2）avformat_open_intput():打开视频文件
3）avformat_find_stream_info():获取视频文件信息
4）avcodec_find_decoder():查找解码器
5）avcode_open2():打开解码器
6）av_read_frame():从输入文件读取一帧压缩数据
7）avcodec_decode_video2():解压一帧压缩数据
8）avcodec_close():关闭解码器
9）avformat_close_input():关闭输入视频文件

FFmpeg数据结构:
AVFormatContext    
AVStream         
AVCodecContext          
AVCodec
AVFrame
AVPacket
AVBitStreamFilterContext
数据结构关系:
AVFormatContext
{
    int nb_streams;
    AVStream *streams[];
};
 
AVStream
{
    AVCodecContext*;
};
 
AVCodecContext
{
    AVCodecID ;
};

PTS和DTS：
DTS:解码时间戳
PTS:显示时间戳
通常PTS和DTS只有在流中有B帧的时候会不同
pts和dts两个参数就 是用来控制视频帧的显示和解码的顺序
注意由于编码算法不同，会存在 在后面显示的视频帧会出去视频文件的前面[dts]，比如：
显示顺序：F1,F2,F3,F4,F5,对应pts1，pts2，pts3，pts4，pts5
解码顺序：F1,F2,F4,F3,F5,对应dts1，dts2，dts4，dts3，dts5

Decoder过程output输入的pts是按正常顺序，即视频正常显示的顺序输出的，如果有B帧，decoder会自动做缓存排序处理。
encoder过程output输出是按dts输出的。

I,P，B帧和PTS，DTS的关系：
基本概念：

I frame ：帧内编码帧 又称intra picture，I 帧通常是每个 GOP（MPEG 所使用的一种视频压缩技术）的第一个帧，经过适度地压缩，做为随机访问的参考点，可以当成图象。I帧可以看成是一个图像经过压缩后的产物。

P frame: 前向预测编码帧 又称predictive-frame，通过充分将低于图像序列中前面已编码帧的时间冗余信息来压缩传输数据量的编码图像，也叫预测帧；

B frame: 双向预测内插编码帧 又称bi-directional interpolated prediction frame，既考虑与源图像序列前面已编码帧，也顾及源图像序列后面已编码帧之间的时间冗余信息来压缩传输数据量的编码图像，也叫双向预测帧；

PTS：Presentation Time Stamp。
PTS主要用于度量解码后的视频帧什么时候被显示出来

DTS：Decode Time Stamp。
DTS主要是标识读入内存中的bit流在什么时候开始送入解码器中进行解码。

在没有B帧存在的情况下DTS的顺序和PTS的顺序应该是一样的。

ＩＰＢ帧的不同：

I frame:自身可以通过视频解压算法解压成一张单独的完整的图片。

P frame：需要参考其前面的一个I frame 或者B frame来生成一张完整的图片。

B frame:则要参考其前一个I或者P帧及其后面的一个P帧来生成一张完整的图片。

两个I frame之间形成一个GOP，在x264中同时可以通过参数来设定bf的大小，即：I 和p或者两个P之间B的数量。

通过上述基本可以说明如果有B frame 存在的情况下一个GOP的最后一个frame一定是P.

DTS和PTS的不同：

DTS主要用于视频的解码,在解码阶段使用.
PTS主要用于视频的同步和输出.在display的时候使用.在没有B frame的情况下.DTS和PTS的输出顺序是一样的.

音频压缩算法：
WAV、MP3、OGG
AAC: 主要有 LC-AAC、HE-AAC、HE-AAC v2 三种编码格式，分别应用于中高码率(>= 80kbit/s)、中低码率(<= 80kbit/s)、低码率(<= 48kbit/s)场景。

比特率：
    比特率指每秒传送的比特数，单位为 bps(Bit Per Second)，比特率越高，传送数据速度越快。声音中的比特率是指将模拟声音信号转换成数字声音信号后，单位时间内的二进制数据量，是间接衡量音频质量的一个指标。
多媒体行业在指音频或者视频在单位时间内的数据传输率时通常使用码率，单位是 kbps(千位每秒)。视频中的码率的概念与声音中的相同，都是指由模拟信号转换为数字信号后，单位时间内的二进制数据量。比如 1.44Mbps，就是 1 秒内到达的数据量为 1.44Mb（注意，是 bit，不是 byte）。

音频中比特率的计算公式如下：
    比特率 = 采样率 * 采样精度 * 声道数目
视频中比特率的计算公式如下：
    比特率 = 帧率 * 每帧数据大小

RGB：
任何一幅图像都可以由 RGB 组成，常用的 RGB 表示法有两种：
1、浮点表示，取值范围为 0.0 ~ 1.0，OpenGL ES 使用的就是这种方式
2、整数表示，取值范围为 0 ~ 255 或者 00 ~ FF，很多平台用的都是这种表达方式，比如 RGB_565 就使用了 16bit 来表示一个像素，R 用 5bit，G 用 6bit，B 用 5bit。

YUV：
对于视频的裸数据而言，用得更多的是 YUV 数据格式
其中 “Y” 表示明亮度（Luminance 或 Luma），即灰度值；而 “U” 和 “V” 表示的则是色度（Chrominance 或 Chroma），作用是描述影像色彩及饱和度，指定像素的颜色。
YUV 主要用于电视系统以及模拟视频领域，它将亮度信息（Y）与色彩信息（UV）分离，没有 UV 信息一样可以显示完整的图像，只不过是黑白的，这样的设计很好地解决了彩色电视机与黑白电视的兼容问题。并且，YUV 不像RGB 那样要求三个独立的视频信号同时传输，所以用 YUV 数据占用的内容更少。

RGB转YUV[从 ITU-R BT.601-7 标准中可以拿到推荐的相关系数]
Y = 0.299R + 0.587G + 0.114B
Cb = 0.564(B - Y)
Cr = 0.713(R - Y)
YCbCr 是属于 YUV 家族的一员（Cb、Cr 的含义等同于 U、V ），是在计算机系统中应用最为广泛的颜色模型。在 YCbCr 中，Y 是指亮度分量，Cb 指蓝色色度分量，而 Cr 指红色色度分量。
YCbCr转RGB：
R = Y + 1.402Cr
G = Y - 0.344Cb - 0.714Cr
B = Y + 1.772Cb

YUV 格式有两大类：planar 和 packed。planar 先存储所有 Y，紧接着存储所有 U，最后是 V；而 packed 则是每个像素点的 Y、U、V 连续交叉存储。packed 格式已经很少使用了，现在几乎都用planar格式。planar 的意思是平面，一个 planar 存储一个分量，因此 YUV 需要三个 planar 来存储图像信息。

stride：
    指内存中每行像素所占的空间。为了实现内存对齐，每行像素在内存中所占的空间不一定是图像的宽度。因此 stride 大于等于图像帧的宽度，同时，stride 为 4 的倍数

图像编码：
    ISO 制定的 MPEG2、MPEG4 标准，ITU-T 的 H.261、H.262、H.263、H.264、H.265，Google 的 VP8、VP9 ， AOM 的 AV1 等。其中 H265(HEVC)、VP9、AV1 是最新的几个编解码标准/格式，现在用得最多的还是 H264。

H264 又称为 MPEG-4 Part 10, Advanced Video Coding，简写为 AVC。因为 ITU-T H.264 标准和 ISO/IEC MPEG-4 AVC 标准（正式名称是ISO/IEC 14496-10—MPEG-4 第十部分，高级视频编码）有相同的技术内容，故被共同管理。
H264 的相关概念有：序列、图像、片组、片、NALU、宏块、亚宏块、块、像素。

帧内预测、帧间预测：
    视频编码中，最常用的数据压缩算法是帧内预测、帧间预测。帧内预测是对单张图像本身进行数据压缩，比如 JPEG。帧间预测是利用视频图像帧间的相关性，来达到图像压缩的目的；因为同一个视频中的前后两帧图像差异很小，因此不必完整地保存这两帧图像的原始数据，只需要保存前一帧的图像数据，然后再保存后一帧画面与前一帧的差别即可。

IPB帧、IDR帧：
    根据帧间预测算法，对应的帧类型有 I 帧（intra picture）、P 帧（predictive-frame）、B 帧（bi-directional interpolated prediction frame）。I 帧是关键帧，使用帧内预测，无需借助其它帧的信息即可完整地呈现出一幅图像；P 帧没有完整的图像数据，只保存与前面的帧的差别，需要借助之前的帧数据生成图像；B 帧记录的是与前后帧的差别。

    此外还有一个概念为 IDR 帧（instantaneous decoding refresh picture），因为 I 帧后的 P 帧可能会参考 I 帧之前的帧，这使得在随机访问的时候，可能即使找到了 I 帧，后面的 P 帧也无法解码。因此，IDR 会清空参考帧列表（DPB），IDR 帧之后的帧都不能引用任何 IDR 帧之前的帧数据。

片、NAL、宏块:
H.264 的主要目标有两个：高视频压缩比、良好的网络亲和性。为此， H.264 的功能分为两层，即视频编码层（VCL）和网络提取层（NAL， Network Abstraction Layer）。 VCL 数据即编码处理的输出，它表示被压缩编码后的视频数据序列。在 VCL 数据传输或存储之前，这些编码的 VCL 数据，会被映射或封装进 NAL Unit 中。每个 NAL Unit 包括一个原始字节序列负荷（RBSP）、一组对应于视频编码数据的 NAL 头信息[NAL:RBSP,NAL:RBSP,NAL:RBSP...]。

NAL Unit 的头占一个字节，由三部份組成，包括 forbidden_bit、nal_reference_idc 和 nal_unit_type。其中 forbidden_bit 占 1 bit，一般来说其值为 0；nal_reference_idc 占 2 bit，用于表示此 NAL 在重建过程中的重要程度。剩下 5 bit 表示 nal_unit_type，用于表示该 NAL Unit （RBSP）的类型。

H264 vs x264
    H264 是一个标准，一种格式，定义了视频流应该如何被压缩编码
    x264 是一个开源的编码器，用于产生 H264 格式的视频流
h264 vs avc
    属于 MP4 封装的 H264 视频的两种格式，都属于“H264”，只有一个区别：
    h264：带起始码 0x00 00 01 或 0x00 00 00 01
    avc：不带起始码

常用软件
MediaInfo：
    用于查看视频参数
ffmpeg三个命令行工具：
    ffplay：用于播放音视频，包括 yuv、pcm 等裸数据
    ffprobe：用于查看媒体文件头信息
    ffmpeg：强大的媒体文件转换工具，还可以转换图片格式
    YUVPlayer：播放 yuv 裸数据
    VLC：多媒体播放器

YUV转换为RGB使用OpenGL效率会比较高





