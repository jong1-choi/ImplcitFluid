# ImplcitFluid

 Real-Time Fluid Dynamics for Games [jos stam] 참고
 
 
<img src="https://github.com/jong1-choi/ImplcitFluid/blob/main/Images/demo.gif" width="400" height="400">

### 구현 방법
안정성을 위해 2차원 그리드에서 implicit euler method를 사용하였고, jacobi iteration을 통해 밀도와 속도에 대해 Naver Strokes equation을 풀어 문제를 해결합니다.

먼저 Explicit한 방법을 사용했을 때 발생하는 문제점입니다.

Explicit Diffuse Term

<img src="https://github.com/jong1-choi/ImplcitFluid/blob/main/Images/image1.gif>

<img src="https://github.com/jong1-choi/ImplcitFluid/blob/main/Images/image2.gif>

Explicit Advection term

<img src="https://github.com/jong1-choi/ImplcitFluid/blob/main/Images/image3.gif>

Time step의 크기에 따라 불안정해지는 것을 확인 할 수 있습니다.

이러한 안정성의 이유로 Implicit한 방법을 사용한 diffuse term과 advection term에 댜해 각각 설명을 드리겠습니다.

먼저 diffuse term은 2차 편미분값에 확산률이 곱해진 형태로 implcit한 방법을 사용했기 때문에 다음 타임 스텝의 값에서 diffuse term으로 인한 변화를 빼주면 현재 값이 나오게 됩니다. 다음 타임스텝의 값을 모르지만 이러한 형태를 선형 시스템으로 표현하면 행렬 A의 형태가 strictly dominant diagonal의 형태가 되므로 특정 횟수만큼 jacobi iteration을 사용하여 값을 근사합니다.

<img src="https://github.com/jong1-choi/ImplcitFluid/blob/main/Images/image4.gif>

<img src="https://github.com/jong1-choi/ImplcitFluid/blob/main/Images/image5.gif>

<img src="https://github.com/jong1-choi/ImplcitFluid/blob/main/Images/image6.gif>

또 advection term은 implicit한 접근으로 각 그리드에서 타임 스텝 만큼의 속도를 빼주면 이전의 해당 부분이 어느 그리드에서 왔는지 알 수 있기 때문에 해당 그리드 주변의 값들을 bi-linear interpolation을 통해 구하고 저장해 두었다가 모든 그리드에서 대해 계산이 완료되면 업데이트 합니다.

<img src="https://github.com/jong1-choi/ImplcitFluid/blob/main/Images/image7.gif>

<img src="https://github.com/jong1-choi/ImplcitFluid/blob/main/Images/image8.gif>

<img src="https://github.com/jong1-choi/ImplcitFluid/blob/main/Images/image9.gif>

이러한 식을 밀도와 속도에 대해 각각 계산하여 업데이트 시켜줌으로써 stable fluids를 표현합니다.
