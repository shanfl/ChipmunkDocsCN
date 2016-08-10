##2d
[toc]
##### cpBodyType
 * CP_BODY_TYPE_DYNAMIC: 默认类型,受重力,力和碰撞的影响
 * CP_BODY_TYPE_KINEMATIC :　1.无限质量,不受重力,外力,和碰撞的影响,2.相反物体根据自身速度而移动,3.动态刚体可与其发生碰撞而影响,KINEMATIC类型的刚体不受影响.4.两个KINEMATIC物体间,或者KINEMATIC物体与静态物体间的碰撞发生碰撞回调,但没有碰撞反应
 * CP_BODY_TYPE_STATIC　:　静态刚体不会移动．如果你手动移动静态刚体，你需要调用cpSpaceReindex*()系列函数.2个静态刚体之间不产生碰撞回调

#####约束
 * cpDampedSpringNew
 > cpConstraint* cpDampedSpringNew(cpBody *a, cpBody *b, cpVect anchorA, cpVect anchorB, cpFloat restLength, cpFloat stiffness, cpFloat damping);
 > 参数解释: restLength : 弹簧长度, stiffness  : 弹簧刚度,  damping :阻尼系数,此数值很大,弹簧长度会不发生变化

##### 转动惯量
 * 转动惯量

#####cpCollisionHandler

> 
```[cpp]
/// Struct that holds function callback pointers to configure custom collision handling.
/// Collision handlers have a pair of types; when a collision occurs between two shapes that have these types, the collision handler functions are triggered.
struct cpCollisionHandler {
	/// Collision type identifier of the first shape that this handler recognizes.
	/// In the collision handler callback, the shape with this type will be the first argument. Read only.
	const cpCollisionType typeA;
	/// Collision type identifier of the second shape that this handler recognizes.
	/// In the collision handler callback, the shape with this type will be the second argument. Read only.
	const cpCollisionType typeB;
	/// This function is called when two shapes with types that match this collision handler begin colliding.
	cpCollisionBeginFunc beginFunc;
	/// This function is called each step when two shapes with types that match this collision handler are colliding.
	/// It's called before the collision solver runs so that you can affect a collision's outcome.
	cpCollisionPreSolveFunc preSolveFunc;
	/// This function is called each step when two shapes with types that match this collision handler are colliding.
	/// It's called after the collision solver runs so that you can read back information about the collision to trigger events in your game.
	cpCollisionPostSolveFunc postSolveFunc;
	/// This function is called when two shapes with types that match this collision handler stop colliding.
	cpCollisionSeparateFunc separateFunc;
	/// This is a user definable context pointer that is passed to all of the collision handler functions.
	cpDataPointer userData;
};```


#####形状

 * cpShapeSetSensor(cpShape*shape,cpBool value);
 > 对形状设置Sensor属性,传感器只调用碰撞回调,但不产生真是的碰撞.
 * cpShapeSetCollisionType(cpShape*shape,cpCollisionType type);
 > 为碰撞形状指定类型,从而在接触特定类型物体的时候触发回调


----
#####碰撞过滤

2种忽略碰撞的方式

###### 群组
- cpShapeSetGroup
- 相同群组的形状不产生碰撞


###### 层
- cpShapeSetLayers
- 形状可以隶属于一个或者多个层
- 相同层的shape才可以产生碰撞,不同层的shape 不碰撞

---

##### 操作运算
> cpVect运算方法 

- cpBool cpveql(const cpVect v1, const cpVect v2) – 检测两个向量是否相等。在使用C++程序时，Chipmunk提供一个重载操作符`==`。（比较浮点数时要小心！）
- cpVect cpvadd(const cpVect v1, const cpVect v2) – 两个向量相加。在使用C++程序时，Chipmunk提供一个重载操作符`+`。
- cpVect cpvsub(const cpVect v1, const cpVect v2) – 两个向量相减。在使用C++程序时，Chipmunk提供一个重载操作符`-`。
- cpVect cpvneg(const cpVect v) – 使一个向量反向。在使用C++程序时，Chipmunk提供一个重载一个一元负操作符`-`。
- cpVect cpvmult(const cpVect v, const cpFloat s) – 标量乘法。在使用C++程序时，Chipmunk提供一个重载操作符`*`。
- cpFloat cpvdot(const cpVect v1, const cpVect v2) – 向量的点积。
- cpFloat cpvcross(const cpVect v1, const cpVect v2) – 2D向量交叉相乘的模。2D向量交叉相乘的积作为一个只有z坐标的3D向量的z值。函数返回z坐标的值。
- cpVect cpvperp(const cpVect v) – 返回一个垂直向量。（旋转90度）
- cpVect cpvrperp(const cpVect v) – 返回一个垂直向量。（旋转-90度）
- cpVect cpvproject(const cpVect v1, const cpVect v2) – 返回向量v1在向量v2上的投影。
- cpVect cpvrotate(const cpVect v1, const cpVect v2) – 使用复杂的乘法运算将向量v1按照向量v2旋转。如果v1不是单位向量，则v1会被缩放。
- cpVect cpvunrotate(const cpVect v1, const cpVect v2) – 和cpvrotate()相反。
- cpFloat cpvlength(const cpVect v) – 返回v的长度。
- cpFloat cpvlengthsq(const cpVect v) – 返回v的长度的平方，如果只是比较长度的话它的速度比cpvlength()快。
- cpVect cpvlerp(const cpVect v1, const cpVect v2, const cpFloat t) – 在v1和v2之间线性插值。
- cpVect cpvlerpconst(cpVect v1, cpVect v2, cpFloat d) – 以长度d在v1和v2之间线性插值。
- cpVect cpvslerp(const cpVect v1, const cpVect v2, const cpFloat t) – 在v1和v2之间球形线性插值。
- cpVect cpvslerpconst(const cpVect v1, const cpVect v2, const cpFloat a) – 在v1和v2之间以不超过角a的弧度值球形线性插值。
- cpVect cpvnormalize(const cpVect v) – 返回a的一个归一化副本。作为特殊例子，在调用cpvzero时返回cpvzero。
- cpVect cpvclamp(const cpVect v, const cpFloat len) – 将v固定到len上。
- cpFloat cpvdist(const cpVect v1, const cpVect v2) – 返回v1和v2间的距离。
- cpFloat cpvdistsq(const cpVect v1, const cpVect v2) – 返回v1和v2间的距离的平方。如果只是比较距离的话它比cpvdist()快。
- cpBool cpvnear(const cpVect v1, const cpVect v2, const cpFloat dist) – 如果v1和v2间的距离小于dist则返回真。
- cpVect cpvforangle(const cpFloat a) – 返回所给角（以弧度）单位向量。
- cpFloat cpvtoangle(const cpVect v) – 返回v所指的角度方向的弧度。

>  数学运算

-  cpFloat cpfclamp(cpFloat f, cpFloat min, cpFloat max) - 截断f在min和max之间
-  cpFloat cpflerp(cpFloat f1, cpFloat f2, cpFloat t) - 对f1和f2进行线性插值
-  cpFloat cpflerpconst(cpFloat f1, cpFloat f2, cpFloat d) - 从f1到f2不超过d的线性插值

> 包围盒运算方法

-  cpBool cpBBIntersects(const cpBB a, const cpBB b) - 如果边界框相交返回true
-  cpBool cpBBContainsBB(const cpBB bb, const cpBB other) - 如果`bb`完全包含`other`返回true
-  cpBool cpBBContainsVect(const cpBB bb, const cpVect v) - 如果`bb`包含`v`返回true
-  cpBB cpBBMerge(const cpBB a, const cpBB b) - 返回包含`a`和`b`的最小的边界框
-  cpBB cpBBExpand(const cpBB bb, const cpVect v) - 返回包含`bb`和`v`的最小的边界框
-  cpVect cpBBCenter(const cpBB bb) - 返回`bb`的中心点矢量
-  cpFloat cpBBArea(cpBB bb) - 返回`bb`矢量表示的边界框的面积
-  cpFloat cpBBMergedArea(cpBB a, cpBB b) - 合并`a`和`b`然后返回合并后的矢量的边界框的面积
-  cpFloat cpBBSegmentQuery(cpBB bb, cpVect a, cpVect b) - 返回分段查询相交`bb`的相交点个数，如果没有相交，返回`INFINITY`
-  cpBool cpBBIntersectsSegment(cpBB bb, cpVect a, cpVect b) - 如果由`a`和`b`两端点定义的线段和`bb`相交返回true
-  cpVect cpBBClampVect(const cpBB bb, const cpVect v) - 返回`v`在边界框中被截断的矢量的副本
-  cpVect cpBBWrapVect(const cpBB bb, const cpVect v) - 返回`v`包含边界框的矢量的副本
------