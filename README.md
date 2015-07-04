# leapmotion_first_project
# Rotation and Destruction
using UnityEngine;
using System.Collections;
using Leap;

public class GestureHands : MonoBehaviour {
	Controller controller;

	public GameObject blockOne;
	public GameObject blockTwo;
	public GameObject blockThree;

	public bool spinLeft;
	public bool spinRight;
	public bool DesUp;
	public bool DesDown;
	// Use this for initialization
	void Start () {
		controller = new Controller ();
		controller.EnableGesture (Gesture.GestureType.TYPESWIPE);
		controller.Config.SetFloat ("Gesture.Swipe.MinLength", 200.0f);
		controller.Config.SetFloat ("Gesture.Swipe.MinVelocity", 750f);
		controller.Config.Save ();

	}
	
	// Update is called once per frame
	void Update () {
		Frame frame = controller.Frame ();
		GestureList gestures = frame.Gestures ();
		Hand hand = frame.Hands.Frontmost;
		if (hand.IsRight) {
			for (int i = 0; i<gestures.Count; i++) {
				Gesture gesture = gestures [i];
				if (gesture.Type == Gesture.GestureType.TYPESWIPE) {
					SwipeGesture Swipe = new SwipeGesture (gesture);


					Vector swipeDirection = Swipe.Direction;
					if (swipeDirection.x < 0) {
						Debug.Log ("Left");
						//DestroyObject (blockOne);
						spinLeft = true;
						spinRight = false;
					
					}
			
					if (swipeDirection.x > 0) {

						Debug.Log ("Right");
						//DestroyObject (blockTwo);
						spinRight = true;
						spinLeft = false;
					}
			
				}
			}
		}
		if (hand.IsLeft) {
			for (int j = 0; j<gestures.Count; j++) {
				Gesture gesture = gestures [j];
				if (gesture.Type == Gesture.GestureType.TYPESWIPE) {
					SwipeGesture Swipe = new SwipeGesture (gesture);
					Vector swipeDirection = Swipe.Direction;
					if (swipeDirection.x < 0) {
						Debug.Log ("Left Hand Destroys Stuffs!");
						spinLeft = true;
						spinRight = false;
						DesUp = true;
						DesDown = false;
						
					}
					if (swipeDirection.x > 0) {
						Debug.Log ("Left Hand Destroys Stuffs!");
						spinLeft = false;
						spinRight = true;
						DesDown = true;
						DesUp = false;
						
					}
				}
			}
		}
	

				if (spinLeft == true) {
					blockOne.transform.Rotate (Vector3.left, 45 * Time.deltaTime);
				}
				if (spinRight == true) {
					blockOne.transform.Rotate (Vector3.right, 45 * Time.deltaTime);
				}
				if (DesUp == true) {
					DestroyObject (blockTwo);
				}
				if (DesDown == true) {
					DestroyObject (blockThree);
				}
			}	
		}
