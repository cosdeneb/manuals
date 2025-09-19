# manuals
## 1. How to use Isaac Sim?

Issac Lab 사용법

[step 1]
./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py --task=Isaac-Ant-v0 --headless
여기서, --headless는 학습 과정을 Isaac Sim에서 보지 않고 Terminal상에서만 보고자 할 때 사용

[step 2]
학습이 진행되면, /home/chanyoung/IsaacLab/logs/rsl_rl/ant/ 경로에 .pt파일이 저장됨

[step 3]
./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/play.py \
--task=Isaac-Ant-v0 \
--checkpoint=/home/chanyoung/IsaacLab/logs/rsl_rl/ant/2025-08-28_16-54-42/model.pt
위의 명령어를 통해, deployment를 시켜볼수 있음!

빈 Issac sim 프로젝트를 하나 가져오고자 할때:
cd ./isaacsim 에 들어간 뒤, ./isaac-sim.selector.sh --base 입력하면 완료

만약 Isaac Lab환경을 사용하고자 한다면,
(1) conda activate env_isaaclab
(2) cd IsaacLab
(3. 빈 프로젝트 파일) ./isaaclab.sh -p scripts/tutorials/00_sim/create_empty.py
(3-1. 4족 보행 로봇 시뮬레이션) ./isaaclab.sh -p scripts/demos/quadrupeds.py
위 3단계를 순차적으로 진행해주면 된다.

참고: https://isaac-sim.github.io/IsaacLab/v2.0.2/source/setup/installation/binaries_installation.html#installing-isaac-sim

## 2. How to use both ROS1 and ROS2?

ROS 사용법

[ROS1]
ros1_on : Activating ROS1 (Noetic) … 뜸
이후 roscore로 실행하면 됨

[ROS2]
ros2_on : Activating ROS2 (Foxy) … 뜸
(터미널 1) ros2 run demo_nodes_cpp talker 
			+ 
(터미널 2) ros2 run demo_nodes_cpp listener 로 통신상태 확인

bash.rc를 수정하기 위해서는 gedit ~/.bashrc 입력하면 되고, source ~/.bashrc 로 실행 가능

[아래는 .bashrc 하단에 추가한 코드와 같음 ]
ROS 환경을 위한 함수
function ros1_on() {
  echo "Activating ROS1 (Noetic)..."
  source /opt/ros/noetic/setup.bash
}

function ros2_on() {
  echo "Activating ROS2 (Foxy)..."
  source /opt/ros/foxy/setup.bash
}

참고: https://qlalf-smithy.tistory.com/18

*ROS2 workspace 만드는법

[step 1]
mkdir -p ~/WORK_SPACE_NAME/src && cd ~/WORK_SPACE_NAME/src를 하여 폴더에 들어감

[step 2]
터미널에 git clone https://github.com/ros/ros_tutorials.git -b foxy-devel를 작성하면, 
ros_tutorials폴더 내에
(1) roscpp_tutorials (2) rospy_tutorials (3) ros_tutorials (4) turtlesim 폴더가 생기는 것을 확인

[step 3]
위 4개의 폴더에 종속성이 있는지 자동으로 확인해주는 CLI 명령어를 사용

cd ../..를 통해 ./WORK_SPACE_NAME 으로 가서,
rosdep install -i --from-path src --rosdistro foxy -y
> All required rosdeps installed successfully가 뜨는 것을 확인!

[step 4]
마지막으로, colcon을 통한 (turtlesim)빌드를 진행
colcon build --symlink-install
ls
> build install log src

이 때, gedit ~/.bashrc 내부에 들어가서 제일 하단에 아래 코드를 붙여넣으면, 일일히 colcon을 안써도 됨
alias cba='colcon build --symlink-install'
alias cbp='colcon build --symlink-install --packages-select'
alias killg='killall -9 gzserver && killall -9 gzclient && killall -9 rosmaster'
alias rosfoxy='source /opt/ros/foxy/setup.bash && source ~/gcamp_ros2_ws/install/local_setup.bash'

맨 아래 한 줄은 미리 정의해둔 ros2_on() 함수를 적용하면 될듯!

[step 5]
마지막으로, 터미널을 재실행한 뒤에 cd/WORK_SPACE_NAME 들어가서 ros2_on켜주고,
(터미널 1) ros2 run turtlesim turtlesim_node

(터미널 2) ros2 run turtlesim turtle_teleop_key
로 거북이를 조작할 수 있으면 성공적으로 설치가 완료된 것임!

참고: https://puzzling-cashew-c4c.notion.site/ROS-2-Foxy-Linux20-04-58f0c6f2537e498eb8fe163ad1f13ce5
