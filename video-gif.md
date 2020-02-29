## Need
Convert .mov/.MP4 to .gif
  
### Reason
As a developer, I feel better to upload a short video when I create the pull request to show other viewers what I did in this PR. I tried .mov format directly got after finishing recording screen using Quicktime, however, gif offers preview in most web pages, and has smaller file size.

This is not limited to developer, anyone has this need can use this method to convert the files.
  
### Thoughts
I tried to use some software to do the job, but my hands are tied since I don't have admin in my office comupter, but I do have brew installed. And I think using command line tool will be a good choice. So I will use *HomeBrew*, *ffmpeg*, *gifsicle* to do the job.
  
### Solution
  1. download HomeBrew
  2. `$brew install ffmpeg`
  3. `$brew install gifsicle`
  4. `$ffmpeg -i in.mov -pix_fmt rgb8 -r 10 output.gif && gifsicle -O3 output.gif -o output.gif`

### Explanation 
  1. Convert the file to gif using `ffmpeg` 
  
    - input path argument `-i`
    - pixel format argument `-pix_fmt`
    - removing some frames using framerate argument `-r`
    - end `ffmpeg` with new path/to/filename
  2. Optimize the same output file with third option `-O3` and rewrite the generated gif file from last step
  3. Notes: using `&&` to make sure the conversion sucess before optimizing
  
### References
  1. https://brew.sh
  2. https://www.ffmpeg.org
  3. https://www.lcdf.org/gifsicle/
  4. full video tutorial with explanation https://www.youtube.com/watch?v=QKtRMFvvDL0
  