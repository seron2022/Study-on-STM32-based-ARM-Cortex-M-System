对于结构体struct
常见的代码格式有：

typedef struct
{
    int16_t Accel_X_RAW;
    int16_t Accel_Y_RAW;
    int16_t Accel_Z_RAW;

    double Ax;
    double Ay;
    double Az;

    int16_t Gyro_X_RAW;
    int16_t Gyro_Y_RAW;
    int16_t Gyro_Z_RAW;

    double Gx;
    double Gy;
    double Gz;

    float Temperature;

    double KalmanAngleX;
    double KalmanAngleY;
} MPU6050_t;

它的作用可以理解为：

把一堆互相关联的数据打包成一个新的数据类型。

于是，后续可以直接用：

MPU6050_t imu;
定义了imu这个变量

也可以说，后续
MPU6050_t a;
MPU6050_t b;
MPU6050_t c;
a,b,c这几个变量都将以MPU6050_t里面的格式呈现。

另一种写法：

struct MPU6050 
{
    int16_t Accel_X_RAW;
    int16_t Accel_Y_RAW;
    int16_t Accel_Z_RAW;

    double Ax;
    double Ay;
    double Az;

    int16_t Gyro_X_RAW;
    int16_t Gyro_Y_RAW;
    int16_t Gyro_Z_RAW;

    double Gx;
    double Gy;
    double Gz;

    float Temperature;

    double KalmanAngleX;
    double KalmanAngleY;
}

struct MPU6050 imu;

可知typedef可以简化结构体的表达。
