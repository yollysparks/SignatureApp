
   #                                             Canvas 2D. 


In a simple layman's language, a canvas is like a blank place like a blank paper /material where someone can write/paint on.
 A Canvas works for you as a pretense, or interface, to the actual surface upon which your graphics will be drawn — it holds all of your "draw" calls. Via the Canvas, your drawing is actually performed upon an underlying Bitmap, which is placed into the window. To draw something, you need 4 basic components: 
 
•	A canvas object. Very simplified, a Canvas is a logical 2D drawing surface that provides methods for drawing onto a bitmap.
•	An instance of the Bitmap class which represents the physical drawing surface and gets pushed to the display by the GPU.
•	A View instance associated with the bitmap.
•	A Paint object that holds the style and color information about how to draw geometries, text, and on bitmap.

 
You can associate a Canvas with an ImageView and draw on it in response to user actions. This basic implementation of drawing does not require a custom View. You create an app with a layout that includes an ImageView that has a click handler. You implement the click handler in MainActivity to draw on and display the Canvas.

Drawing to a Canvas, is better when your application needs to regularly re-draw itself. The Canvas class has its own set of drawing methods that you can use, like drawBitmap(...), drawPath(...), drawText(...), and many more. Other classes that you might use also have draw()methods. 

 For example, onDraw() method whatever we need to draw, will fall in this method. Because it is drawing, we continuously refresh it. So, we must avoid creating objects in it and also try to keep calculations outside it as much as possible. Hence, whenever we make a change in the view or updates it, it should be reflected and for that we need to redraw it again. 

 

# Creating our android canvas application.

•	Open Eclipse IDE and go to File → New → Project → Android Application Project.

•	Specify the name of the application, the project and the package and then click Next.

•	In the next window, the “Create Activity” option should be checked. The new created activity will be the main activity of your 
        project. Then press Next button.
	
•	Select the “Blank Activity” option and press Next.

•	Create a CanvasView class where we will have the source code for the main activity.

# The code snipets

In this code snippet for canvas view class.

 We set up a new Canvas. This canvas will draw onto the defined Bitmap.
 
@Override
protected void onSizeChanged(int w, int h, int oldw, int oldh) {
	super.onSizeChanged(w, h, oldw, oldh);
	mBitmap = Bitmap.createBitmap(w, h, Bitmap.Config.ARGB_8888);
	mCanvas = new Canvas(mBitmap);
}

Here, we get the x and y event coordinates in order to make our path moves.

@Override
public boolean onTouchEvent(MotionEvent event) {
	float x = event.getX();
	float y = event.getY();

	switch (event.getAction()) {
	case MotionEvent.ACTION_DOWN:
		startTouch(x, y);
		invalidate();
		break;
	case MotionEvent.ACTION_MOVE:
		moveTouch(x, y);
		invalidate();
		break;
	case MotionEvent.ACTION_UP:
		upTouch();
		invalidate();
		break;
	}
	return true;
                   }
We transform the x,y event coordinates into path moves.

private void moveTouch(float x, float y) {

	float dx = Math.abs(x - mX);
	float dy = Math.abs(y - mY);
	if (dx >= TOLERANCE || dy >= TOLERANCE) {
		mPath.quadTo(mX, mY, (x + mX) / 2, (y + mY) / 2);
		mX = x;
		mY = y;
	}
                     }
And by overriding the onDraw event, we draw our path onto the canvas.'

// override onDraw

@Override
protected void onDraw(Canvas canvas) {

	super.onDraw(canvas);
	canvas.drawPath(mPath, mPaint);
} 

The clear Canvas Method is called to clear the drawing, in it we call an invalidate method which forces redraw.

 // for clearing the signature
 
    public void clearCanvas() {
    
        mPath.reset();
        invalidate();
    }

# Summary 
You are creating a view class then extends View. You override the onDraw(). You add the path of where finger touches and moves. You override the onTouch() of this purpose. In your onDraw() you draw the paths using the paint of your choice. You should call invalidate() to refresh the view.


# Creating a layout for the main ( mainActivity)
We are going to make a very simple layout xml for the AndroidCanvasExample.class, that only consists of a FrameLayout that contains the custom CanvasView, the component from the custom class we are going to make in the lines below. I have chosen the FrameLayout for our external layout, because we want to add a “Clear Button” that will clear, invalidate and empty our canvas. This FrameLayout helped us add the Button above the canvas, like placing it on a seperate layout on top.

Open res/layout/activity_main.xml, go to the respective xml tab and this is what we will have

<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:custom="http://schemas.android.com/apk/res-auto"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFFFFF"
    android:orientation="vertical">
    <com.example.yolly.mysignatureapp.CanvasView
        android:id="@+id/signature_canvas"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:textColor="#FFFFFF" />
   <Button
    android:id="@+id/button"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_alignParentStart="true"
    android:layout_alignParentTop="true"
    android:onClick="newFile"
    android:text="New"
    android:visibility="visible"
    tools:text="New" />
   <Button
        android:id="@+id/button1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|left"
        android:onClick="clearCanvas"
        android:text="Clear Canvas" />
………….
</FrameLayout>                                                 

# Conclusion

With the knowledge I have acquired on canvas I intended to use canvas in creating a 2D graphics app. I managed to create a signature app where someone can write his/her signature and clear the canvas . I decided  to build on the signature app because I have had certain instance where I had to print a document and put my signature then scan the document and send, and at that moment my printer was not working so it took time for me to print sign and scan and send the document. With the app it is a matter of seconds no printer or scanner is needed so it saves time and money.  
Though it is not a complete project, but I managed to create the signature app where someone can sign and clear screen just in case one makes a mistake.




    

