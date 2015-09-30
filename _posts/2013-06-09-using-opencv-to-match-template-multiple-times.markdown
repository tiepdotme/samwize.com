---
layout: post
title: "Using OpenCV to match template multiple times"
date: 2013-06-09 02:15
comments: true
categories: [OpenCV, iOS]
---

OpenCV has a [matchTemplate](http://opencv.itseez.com/modules/imgproc/doc/object_detection.html?highlight=matchtemplate#matchtemplate) function that let you seach for matches between an image and a given template.

<!-- more -->

There is a [tutorial](http://docs.opencv.org/doc/tutorials/imgproc/histograms/template_matching/template_matching.html#template-matching) on that.

However, the tutorial falls short. It only explain how to match 1 occurence.

I know the answer is somewhere in the `result`.. But I am a newbie and cannot figure out. Do are [others](http://answers.opencv.org/question/11180/template-matching-with-multiple-occurance/).

I found the best answer from: [OpenCV-Code.com](http://opencv-code.com/quick-tips/how-to-handle-template-matching-with-multiple-occurences/)

The genius part of the code is that it finds a match with `minMaxLoc`, then it `floodFill` it. Then it can repeat.

I have used it for an iOS project to count the number of matching templates. Here's the code:

```cpp
// Returns number of matching templates
+ (int)matchTemplate:(UIImage*)templateImage in:(UIImage*)src {
    // http://opencv-code.com/quick-tips/how-to-handle-template-matching-with-multiple-occurences/
    cv::Mat ref = [src CVMat];
    cv::Mat tpl = [templateImage CVMat];
    cv::Mat gref, gtpl;
    cv::cvtColor(ref, gref, CV_BGR2GRAY);
    cv::cvtColor(tpl, gtpl, CV_BGR2GRAY);
    
    cv::Mat res(ref.rows-tpl.rows+1, ref.cols-tpl.cols+1, CV_32FC1);
    cv::matchTemplate(gref, gtpl, res, CV_TM_CCOEFF_NORMED);
    cv::threshold(res, res, 0.9, 1., CV_THRESH_TOZERO);
    
    int count = 0;
    while (true)
    {
        double minval, maxval, threshold = 0.9;
        cv::Point minloc, maxloc;
        cv::minMaxLoc(res, &minval, &maxval, &minloc, &maxloc);
        
        if (maxval >= threshold)
        {
            cv::rectangle(
                          ref,
                          maxloc,
                          cv::Point(maxloc.x + tpl.cols, maxloc.y + tpl.rows),
                          CV_RGB(0,255,0), 2
                          );
            cv::floodFill(res, maxloc, cv::Scalar(0), 0, cv::Scalar(.1), cv::Scalar(1.));
            count++;
        }
        else
            break;
    }
    
    NSLog(@"# of matches: %d", count);

    return count;
}
```