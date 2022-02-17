## 七种方式实现垂直居中
如果.parent的height不写，你只需要padding: 10px 0; 就能将.child垂直居中；
如果.parent的height写死了，就很难把.child居中，以下是垂直居中的方法。
> 忠告：能不写height就千万别写height。

1. [table自带功能](https://codepen.io/stevenguo99/pen/MWOrmVo)
2. [100%高度的after before加上inline block](https://codepen.io/stevenguo99/pen/YzEYZYY) 
  
    此外还有[优化版本](https://codepen.io/stevenguo99/pen/ExbompL)
3. [div装成table](https://codepen.io/stevenguo99/pen/mdqpwPV)
4. [margin-top -50%](https://codepen.io/stevenguo99/pen/zYPpzNy)
5. [translate -50%](https://codepen.io/stevenguo99/pen/OJOzgzm)
6. [absolute margin auto](https://codepen.io/stevenguo99/pen/yLPpXvx)
7. [flex](https://codepen.io/stevenguo99/pen/KKyZqoe)
8. ...