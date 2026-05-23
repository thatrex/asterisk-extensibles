# Asterisk Apps

> [!IMPORTANT]
> Basic terminal and PBX management skills are assumed.

Extensible apps for asterisk: [**Talk Bot**](/apps/talk-bot/) • [**Music Player**](/apps/music-player/)

# Conversion Script

This script will convert common AV files (`mp3, wav, flac, webm, mkv, mp4`) to **Mono 8kHz PCM WAV** files suitable for Asterisk. Converted files will be placed in a subdirectory named `processed`.

1. Ensure [FFmpeg](https://ffmpeg.org/) is installed on your system.
2. Open your terminal, then navigate to the directory containing the audio files to be converted.
3. Copy and paste the script into your PowerShell terminal.

<h3>PowerShell</h3>

_The text being red is not of concern._

```powershell
$OPATH = 'processed'
$TYPES = 'mp3', 'wav', 'flac', 'webm', 'mkv', 'mp4'

New-Item $OPATH -Type Directory -Force

$TYPES `
| ForEach-Object { Get-ChildItem "*.$_" } `
| ForEach-Object -Parallel {
    $OFILE = Join-Path $using:OPATH ($_.BaseName + '.wav')
    & ffmpeg -i $_.Name -ar 8000 -ac 1 -acodec pcm_s16le $OFILE # -map_metadata -1
} -ThrottleLimit 4
```
