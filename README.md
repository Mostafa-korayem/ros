# ros
import rospy
from std_msgs.msg import String
from my_service.srv import *

rospy.init_node('publisher_node')
pub = rospy.Publisher('my_topic', String, queue_size=10)
rate = rospy.Rate(1)

num = 0

def handle_request(request):
    response = "Happy " + str(request.age) + "-th Birthday!!"
    return response

rospy.Service('my_service', MyService, handle_request)

while not rospy.is_shutdown():
    message = "Month " + str(num)
    pub.publish(message)
    num += 1
    rate.sleep()
    if num == 12:
        rospy.wait_for_service('my_service')
        try:
            handle_request = rospy.ServiceProxy('my_service', MyService)
            age_response = handle_request(AgeRequest(0))
            print(age_response.age_message)
        except rospy.ServiceException as e:
            print("Service call failed: %s"%e)
            import rospy
from std_msgs.msg import String
from my_service.srv import *

rospy.init_node('subscriber_node')

def callback(data):
    rospy.loginfo("Received: %s", data.data)
    if data.data == "Month 12":
        rospy.wait_for_service('my_service')
        try:
            handle_request = rospy.ServiceProxy('my_service', MyService)
            age_response = handle_request(AgeRequest(0))
            print(age_response.age_message)
        except rospy.ServiceException as e:
            print("Service call failed: %s"%e)

rospy.Subscriber('my_topic', String, callback)
rospy.spin()
G67mKyj8HN8EfFkA7wt8KX45spaBycGZHFsy2WCf8Wey
rosrun my_service p1.py 
rosrun my_service p2.py 
