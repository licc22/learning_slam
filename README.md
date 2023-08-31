# learning_slam
by slambook
特征点计算：

Harris角点检测算法：
原始思想：选取一个局部窗口，将这个窗口沿着各个方向移动，计算移动前后窗口内像素的差异的多少进而判断窗口对应的区域是否为角点。
E(u,v)=sum(w(x,y)*(I(x+u,y+v)-I(x,y))^2)，其中E为能量函数，u,v体现窗口的移动，x,y窗口内的像素点坐标，w为权重函数，可以是中值滤波或者是高斯滤波函数。
对I(x+u,y+v)进行泰勒展开，进行线性逼近，得到E(u,v)=[u,v]*M*[u,v].T，其中M为实对称矩阵，可进行标准化。
当M矩阵的两个特征值都较小，表示当前区域为平坦区域；当两个特征值都较大，表示为角点；当一个较大，一个较小时，表示为边。
还可计算响应值R=detM-k(traceM)^2，来判断当前区域，k取0.04~0.06；|R|较小为平坦区域，R<0为边；R>0为角点。

SIFT（Scale-invariant feature transform）角点检测与匹配算法：
关键四步：
1、尺度空间（一个变化尺度的高斯函数与原图像的卷积）极值检测：搜索所有尺度上的图像位置。通过高斯微分函数来识别潜在的对于尺度和旋转不变的兴趣点。
2、关键点定位：在每个候选的位置上，通过一个拟合精细的模型来确定位置和尺度。关键点的选择依据于它们的稳定程度。
3、方向确定：基于图像局部的梯度方向，分配给每个关键点位置一个或多个方向。所有后面的对图像数据的操作都相对于关键点的方向、尺度和位置进行变换。从而提供对于这些变换的不变性。
4、关键点描述：在每个关键点周围的领域内，在选定的尺度上测量图像局部的梯度。这些梯度都变换成为一种表示，这种表示允许比较大的局部形状的变形和光照变化。

Fast角点检测算法：
主要检测局部像素灰度变化明显的地方，以速度快著称。它的思想是：如果一个像素与领域的像素差别较大（过亮或过暗），那么它更可能是角点。相比于其他角点检测算法，FAST只需比较像素亮度的大小，十分快捷。
1、在图像中选取像素p，假设它的亮度为Ip。
2、设置一个阈值T（比如，Ip的20%）。
3、以像素p为中心，选取半径为3的圆上的16个像素点。
4、假如选取的圆上有连续的N个点的亮度大于Ip+T或小于Ip-T，那么像素p可以被认为是特征点（N通常取12，9，11）。
5、循环以上四步，对每一个像素执行相同的操作。

Orb角点检测算法：
在Fast基础之上添加了尺度和旋转的描述。尺度不变性由构建图像金字塔，并在金字塔的每一层上检测角点来实现；而特征的旋转是由灰度质心法实现的。

BRIEF描述子：
BRIEF是一种二进制描述子，其描述向量由许多个0和1组成，这里的0和1编码了关键点附近两个随机像素的大小关系：如果p比q大，则取1，反之就取0.如果我们取了128个这样的p，q，则最后得到128维由0、1组成的向量。
