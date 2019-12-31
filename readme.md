# Opencv.js Template matching 模板匹配

從Python版本改來的

網址如下
https://docs.opencv.org/master/d4/dc6/tutorial_py_template_matching.html

## Python範例
```python
import cv2 as cv
import numpy as np
from matplotlib import pyplot as plt
img_rgb = cv.imread('mario.png')
img_gray = cv.cvtColor(img_rgb, cv.COLOR_BGR2GRAY)
template = cv.imread('mario_coin.png',0)
w, h = template.shape[::-1]
res = cv.matchTemplate(img_gray,template,cv.TM_CCOEFF_NORMED)
threshold = 0.8
loc = np.where( res >= threshold)
for pt in zip(*loc[::-1]):
    cv.rectangle(img_rgb, pt, (pt[0] + w, pt[1] + h), (0,0,255), 2)
cv.imwrite('res.png',img_rgb)
```

## Javascript版本
Javascript沒有類似numpy這樣的數學庫，所以會比較麻煩一點

cv.matchTemplate 的回傳物件也不一樣

把 dst.data32F 塞進

cols*rows大小的二維陣列

並且從這個二維陣列中搜尋

數值大於0.97的位置I和K  如果大於0.97

I和K的數值相當於大圖片中已經匹配的的像素點座標


```js
let src = cv.imread(imgSrcElement);
let templ = cv.imread(templateSrcElement);
let dst = new cv.Mat();
let mask = new cv.Mat();

cv.matchTemplate(src, templ, dst, cv.TM_CCORR_NORMED, mask);

let color = new cv.Scalar(255, 0, 0, 255);

var newDst = [];
var start = 0;
var end = dst.cols;

for (var i = 0; i < dst.rows; i++) {

    newDst[i] = [];
    for (var k = 0; k < dst.cols; k++) {

        newDst[i][k] = dst.data32F[start];

        if (newDst[i][k] > 0.97) {

            let maxPoint = {
                "x": k,
                "y": i
            }
            let point = new cv.Point(k + templ.cols, i + templ.rows);
            cv.rectangle(src, maxPoint, point, color, 1, cv.LINE_8, 0);
        }
        start++;
    }
    start = end;
    end = end + dst.cols;
}
cv.imshow('canvasOutput', src);
```



