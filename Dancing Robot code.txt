#include<GL/glut.h>
#include<conio.h>
#include<Gl/GL.h>
#include<GL/GLU.h>
#include<math.h>
#include<stdlib.h>
#include<stdio.h>
#include<time.h>
#include <windows.h>
#include <mmsystem.h>
#define PI 3.14159
#define TORSO_HEIGHT 5.0
#define TORSO_RADIUS 1.5
#define HEAD_HEIGHT 2.0
#define HEAD_RADIUS 1.5
#define UPPER_ARM_HEIGHT 3.0
#define LOWER_ARM_HEIGHT 2.0
#define UPPER_ARM_RADIUS 0.6
#define LOWER_ARM_RADIUS 0.5
#define LOWER_LEG_HEIGHT 3.0
#define UPPER_LEG_HEIGHT 3.0
#define UPPER_LEG_RADIUS 0.6
#define LOWER_LEG_RADIUS 0.5
#define SHOULDER_RADIUS 0.65
#define JOINT_RADIUS 0.75
GLfloat theta[11] = { 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 180.0, 0.0, 180.0, 0.0 };
GLfloat theta_freq[11];
GLfloat theta_max[11];
GLfloat theta_min[11];
GLfloat mat_ambient[] = { 0.1, 0.1, 0.1, 1.0 };
GLfloat mat_diffuse[] = { 0.7, 0.7, 0.7, 1.0 };
GLfloat mat_specular[] = { 0.4, 0.4, 0.4, 1.0 };
GLfloat mat_shininess = { 100.0 };
//int flag_dance = 0;
GLUquadricObj* t, /// body
* gl, /// glass bot
* h, /// head
* lua, /// left upper aram
* lla, /// left lower aram
* rua, /// right upper aram
* rla, /// right lower aram
* lul, /// left upper leg
* rul, /// right upper leg
* lll, /// left lower leg
* rll; /// right lower leg
int where_to_rotate=0;
void body() {
 glPushMatrix();
 glRotatef(-90.0, 1.0, 0.0, 0.0);
 ///(*obj, base, top, height, slices, stacks)
 gluCylinder(t, TORSO_RADIUS, TORSO_RADIUS * 1.5, TORSO_HEIGHT, 10, 10);
 glPopMatrix();
}
void head() {
 glPushMatrix();
 glTranslatef(0.0, 0.5 * HEAD_HEIGHT, 0.0);
 glScalef(HEAD_RADIUS, HEAD_HEIGHT, HEAD_RADIUS);
gluSphere(h, 1.0, 10, 10);
 glPopMatrix();
}
void glass_bot() {
 glPushMatrix();
 glTranslatef(0.0, 0.5 * HEAD_HEIGHT, 0.075);
 glRotatef(-90.0, 1.0, 0.0, 0.0);
 gluCylinder(gl, HEAD_RADIUS, HEAD_RADIUS, HEAD_HEIGHT / 2, 10, 10);
 glPopMatrix();
}
void waist() {
 glPushMatrix();
 gluCylinder(gl, 1.2 * TORSO_RADIUS, 1.2 * TORSO_RADIUS, TORSO_RADIUS / 2, 10, 
10);
 glPopMatrix();
}
void shoulder_joints()
{
 glPushMatrix();
 glScalef(SHOULDER_RADIUS, SHOULDER_RADIUS, SHOULDER_RADIUS);
 gluSphere(h, 1.0, 10, 10);
 glPopMatrix();
}
void elbow_joints() {
 glPushMatrix();
 glScalef(SHOULDER_RADIUS / 1.2, SHOULDER_RADIUS / 1.2, SHOULDER_RADIUS 
/ 1.2);
 gluSphere(h, 1.0, 10, 10);
 glPopMatrix();
}
void palms() {
 glPushMatrix();
glScalef(SHOULDER_RADIUS / 1.3, SHOULDER_RADIUS / 1.3, SHOULDER_RADIUS 
/ 1.3);
 gluSphere(h, 1.0, 10, 10);
 glPopMatrix();
}
void leg_joints() {
 glPushMatrix();
 glScalef(JOINT_RADIUS, JOINT_RADIUS, JOINT_RADIUS);
 gluSphere(h, 1.0, 10, 10);
 glPopMatrix();
}
void knee_joints()
{
 glPushMatrix();
 glScalef(JOINT_RADIUS, JOINT_RADIUS, JOINT_RADIUS);
 gluSphere(h, 1.0, 10, 10);
 glPopMatrix();
}
void left_upper_arm() {
 gluCylinder(lua, UPPER_ARM_RADIUS * 1.2, UPPER_ARM_RADIUS, 
UPPER_ARM_HEIGHT, 10, 10);
}
void left_lower_arm() {
 gluCylinder(lla, LOWER_ARM_RADIUS * 1.1, LOWER_ARM_RADIUS, 
LOWER_ARM_HEIGHT, 10, 10);
}
void right_upper_arm() {
 glPushMatrix();
 glRotatef(-90.0, 1.0, 0.0, 0.0);
 gluCylinder(rua, UPPER_ARM_RADIUS * 1.2, UPPER_ARM_RADIUS, 
UPPER_ARM_HEIGHT, 10, 10);
 glPopMatrix();
}
void right_lower_arm() {
 glPushMatrix();
 glRotatef(-90.0, 1.0, 0.0, 0.0);
 gluCylinder(rla, LOWER_ARM_RADIUS * 1.1, LOWER_ARM_RADIUS, 
LOWER_ARM_HEIGHT, 10, 10);
 glPopMatrix();
}
void left_upper_leg() {
 glColor3f(1.0, 0.0, 1.0);
 glPushMatrix();
 glRotatef(-120.0, 1.0, 0.0, 0.0);
 gluCylinder(lul, UPPER_LEG_RADIUS * 1.2, UPPER_LEG_RADIUS, 
UPPER_LEG_HEIGHT, 10, 10);
 glPopMatrix();
}
void left_lower_leg()
{
 glColor3f(1.0, 0.0, 0.0);
 glPushMatrix();
 glTranslatef(0.0, -0.25, -UPPER_LEG_HEIGHT / 2);
 glRotatef(-70.0, 1.0, 0.0, 0.0);
 gluCylinder(lll, LOWER_LEG_RADIUS, LOWER_LEG_RADIUS * 1.5, 
LOWER_LEG_HEIGHT, 10, 10);
 glPopMatrix();
}
void right_upper_leg()
{
 glColor3f(1.0f, 0.0f, 1.0f);
 glPushMatrix();
 glRotatef(-120.0, 1.0, 0.0, 0.0);
 gluCylinder(rul, UPPER_LEG_RADIUS * 1.2, UPPER_LEG_RADIUS, 
UPPER_LEG_HEIGHT, 10, 10);
glPopMatrix();
}
void right_lower_leg()
{
 glColor3f(1.0, 0.0, 0.0);
 glPushMatrix();
 glTranslatef(0.0, -0.25, -UPPER_LEG_HEIGHT / 2);
 glRotatef(-70.0, 1.0, 0.0, 0.0);
 gluCylinder(rll, LOWER_LEG_RADIUS, LOWER_LEG_RADIUS * 1.5, 
LOWER_LEG_HEIGHT, 10, 10);
 glPopMatrix();
}
void feets() {
 glPushMatrix();
 glScalef(SHOULDER_RADIUS, SHOULDER_RADIUS, SHOULDER_RADIUS);
 gluSphere(h, 1.2, 10, 10);
 glPopMatrix();
}
void display(void) {
 GLfloat rot_x = 0.0, rot_y = 0.0, rot_z = 0.0;
 glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
 glLoadIdentity();
 glColor3f(0.0, 0.0, 0.0);
 glMaterialfv(GL_FRONT, GL_SPECULAR, mat_specular);
 glMaterialfv(GL_FRONT, GL_AMBIENT, mat_ambient);
 glMaterialfv(GL_FRONT, GL_DIFFUSE, mat_diffuse);
 glMaterialf(GL_FRONT, GL_SHININESS, mat_shininess);
 /// set camera
 gluLookAt(0.0, 0.0, 10.0, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0);
 glTranslatef(0.0, 0.0, 10);
 /// ************************** body*************************
glTranslatef(0.0, -2.0, 0.0);
 glRotatef(theta[0], 0.0, 1.0, 0.0);
 body();
 /// ************************** waist *************************
 glPushMatrix();
 glTranslatef(0.0, 1.0, 0.075);
 glRotatef(-90.0, 1.0, 0.0, 0.0);
 waist();
 glPopMatrix();
 /// ************************** head *************************
 glPushMatrix();
 glTranslatef(0.0, TORSO_HEIGHT + 0.5 * HEAD_HEIGHT, 0.0);
 glRotatef(theta[1], 1.0, 0.0, 0.0);
 glRotatef(theta[2], 0.0, 1.0, 0.0);
 glTranslatef(0.0, -0.5 * HEAD_HEIGHT, 0.0);
 head();
 glass_bot();
 glPopMatrix();
 /// ************************** left and right shoulder joints 
*************************
 glPushMatrix();
 glTranslatef(1.5 * TORSO_RADIUS, 0.9 * TORSO_HEIGHT, 0.0);
 shoulder_joints();
 glTranslatef(-3.0 * TORSO_RADIUS, 0.0, 0.0);
 shoulder_joints();
 glPopMatrix();
 /// ************************** left and right leg joints 
*************************
 glPushMatrix();
 glTranslatef(TORSO_RADIUS, 0.0, 0.0);
 leg_joints();
 glTranslatef(-2 * TORSO_RADIUS, 0.0, 0.0);
 leg_joints();
glPopMatrix();
 /// ************************** whole left arm *************************
 glPushMatrix();
 glTranslatef(-(TORSO_RADIUS + UPPER_ARM_RADIUS), 0.9 * TORSO_HEIGHT, 
0.0);
 glRotatef(theta[3], rot_x, rot_y, rot_z);
 left_upper_arm();
 glTranslatef(0.0, 0.0, UPPER_ARM_HEIGHT);
 elbow_joints();
 glRotatef(theta[4], rot_x, rot_y, rot_z);
 left_lower_arm();
 glTranslatef(0.0, 0.0, LOWER_ARM_HEIGHT);
 palms();
 glPopMatrix();
 /// ************************** whole right arm *************************
 glPushMatrix();
 glTranslatef(TORSO_RADIUS + UPPER_ARM_RADIUS, 0.9 * TORSO_HEIGHT, 0.0);
 glRotatef(theta[5], rot_x, rot_y, rot_z);
 right_upper_arm();
 glTranslatef(0.0, UPPER_ARM_HEIGHT, 0.0);
 elbow_joints();
 glRotatef(theta[6], rot_x, rot_y, rot_z);
 glColor3f(1.0, 1.0, 1.0);
 right_lower_arm();
 glTranslatef(0.0, LOWER_ARM_HEIGHT, 0.0);
 palms();
 glPopMatrix();
 /// ************************** whole left leg *************************
 glPushMatrix();
 glTranslatef(-(TORSO_RADIUS), 0.1 * UPPER_LEG_HEIGHT, 0.0);
 glRotatef(theta[7], rot_x, 0.0, 0.0);
 left_upper_leg();
 glTranslatef(0.0, UPPER_LEG_HEIGHT, -1.5);
knee_joints();
 glTranslatef(0.0, 0.0, 1.5);
 glRotatef(theta[8], 1.0, 0.0, 0.0);
 // glRotatef(theta[8], rot_x, rot_y, rot_z);
 left_lower_leg();
 glTranslatef(0.0, LOWER_LEG_HEIGHT, 0.0);
 feets();
 glPopMatrix();
 /// ************************** whole right leg *************************///
 glPushMatrix();
 glTranslatef((TORSO_RADIUS), 0.1 * UPPER_LEG_HEIGHT, 0.0);
 glRotatef(theta[9], rot_x, 0.0, 0.0);
 right_upper_leg();
 glTranslatef(0.0, UPPER_LEG_HEIGHT, -1.5);
 knee_joints();
 glTranslatef(0.0, 0.0, 1.5);
 glRotatef(theta[10], 1.0, 0.0, 0.0);
 right_lower_leg();
 glTranslatef(0.0, LOWER_LEG_HEIGHT, 0.0);
 feets();
 glPopMatrix();
 /// ************************** end *************************///
 glFlush();
 glutSwapBuffers();
 /// glDisable(GL_TEXTURE_2D);
}
/// Caluculate angle
float getAngle(float freq, float min, float max, float time) {
 return (max - min) * sin(freq * PI * time) + 0.5 * (max + min);
}
static void idle(void) {
 GLfloat seconds = glutGet(GLUT_ELAPSED_TIME) / 1000.0;
srand(time(NULL));
 mat_specular[0] = (rand() % 10) * 0.09;
 mat_specular[1] = (rand() % 10) * 0.06;
 mat_specular[2] = (rand() % 10) * 0.03;
 mat_ambient[0] = (rand() % 10) * 0.09;
 mat_ambient[1] = (rand() % 10) * 0.06;
 mat_ambient[2] = (rand() % 10) * 0.03;
 mat_diffuse[0] = (rand() % 10) * 0.09;
 mat_diffuse[1] = (rand() % 10) * 0.06;
 mat_diffuse[2] = (rand() % 10) * 0.03;
 
 /// moving right arm
 if (where_to_rotate == 1) {
 
 theta[6] = getAngle(theta_freq[6], theta_min[6], theta_max[6], seconds);
 theta[5] = getAngle(theta_freq[5], theta_min[5], theta_max[5], seconds);
 }
 /// moving left arm
 else if (where_to_rotate == 2) {
 theta[3] = getAngle(theta_freq[3], theta_min[3], theta_max[3], seconds);
 theta[4] = getAngle(theta_freq[4], theta_min[4], theta_max[4], seconds);
 }
 /// moving right leg
 else if (where_to_rotate == 3) {
 theta[9] = getAngle(theta_freq[9], theta_min[9], theta_max[9], seconds);
 theta[10] = getAngle(theta_freq[10], theta_min[10], theta_max[10], seconds);
 }
 /// moving left leg
 else if (where_to_rotate == 4) {
 theta[7] = getAngle(theta_freq[7], theta_min[7], theta_max[7], seconds);
 theta[8] = getAngle(theta_freq[8], theta_min[8], theta_max[8], seconds);
 }
 /// moving body 
else if (where_to_rotate == 5) {
 theta[0] = getAngle(theta_freq[0], theta_min[0], theta_max[0], seconds);
 }
 /// moving head
 else if (where_to_rotate == 6) {
 theta[1] = getAngle(theta_freq[1], theta_min[1], theta_max[1], seconds);
 }
 else if (where_to_rotate == 7) {
 theta[2] = getAngle(theta_freq[2], theta_min[2], theta_max[2], seconds);
 }
 //moving body with left arm
 else if (where_to_rotate == 8) {
 theta[0] = getAngle(theta_freq[0], theta_min[0], theta_max[0], seconds);
 theta[3] = getAngle(theta_freq[3], theta_min[3], theta_max[3], seconds);
 theta[4] = getAngle(theta_freq[4], theta_min[4], theta_max[4], seconds);
 }
 //moving body with right arm
 else if (where_to_rotate == 9) {
 theta[0] = getAngle(theta_freq[0], theta_min[0], theta_max[0], seconds);
 theta[6] = getAngle(theta_freq[6], theta_min[6], theta_max[6], seconds);
 theta[5] = getAngle(theta_freq[5], theta_min[5], theta_max[5], seconds);
 }
 //moving body with left leg
 else if (where_to_rotate == 10) {
 theta[0] = getAngle(theta_freq[0], theta_min[0], theta_max[0], seconds);
 theta[7] = getAngle(theta_freq[7], theta_min[7], theta_max[7], seconds);
 theta[8] = getAngle(theta_freq[8], theta_min[8], theta_max[8], seconds);
 }
 //moving body with right leg
 else if (where_to_rotate == 11) {
 theta[0] = getAngle(theta_freq[0], theta_min[0], theta_max[0], seconds);
 theta[9] = getAngle(theta_freq[9], theta_min[9], theta_max[9], seconds);
 theta[10] = getAngle(theta_freq[10], theta_min[10], theta_max[10], seconds);
}
 //moving body with head
 else if (where_to_rotate == 12) {
 theta[0] = getAngle(theta_freq[0], theta_min[0], theta_max[0], seconds);
 theta[1] = getAngle(theta_freq[1], theta_min[1], theta_max[1], seconds);
 theta[2] = getAngle(theta_freq[2], theta_min[2], theta_max[2], seconds);
 }
 //moving body with both arms
 else if (where_to_rotate == 13) {
 theta[0] = getAngle(theta_freq[0], theta_min[0], theta_max[0], seconds);
 theta[3] = getAngle(theta_freq[3], theta_min[3], theta_max[3], seconds);
 theta[4] = getAngle(theta_freq[4], theta_min[4], theta_max[4], seconds);
 theta[0] = getAngle(theta_freq[0], theta_min[0], theta_max[0], seconds);
 theta[6] = getAngle(theta_freq[6], theta_min[6], theta_max[6], seconds);
 theta[5] = getAngle(theta_freq[5], theta_min[5], theta_max[5], seconds);
 }
 //moving body with both legs
 else if (where_to_rotate == 14) {
 theta[0] = getAngle(theta_freq[0], theta_min[0], theta_max[0], seconds);
 theta[7] = getAngle(theta_freq[7], theta_min[7], theta_max[7], seconds);
 theta[8] = getAngle(theta_freq[8], theta_min[8], theta_max[8], seconds);
 theta[9] = getAngle(theta_freq[9], theta_min[9], theta_max[9], seconds);
 theta[10] = getAngle(theta_freq[10], theta_min[10], theta_max[10], seconds);
 }
 //moving body with left arm,left leg and head
 else if (where_to_rotate == 15) {
 theta[0] = getAngle(theta_freq[0], theta_min[0], theta_max[0], seconds);
 theta[3] = getAngle(theta_freq[3], theta_min[3], theta_max[3], seconds);
 theta[4] = getAngle(theta_freq[4], theta_min[4], theta_max[4], seconds);
 theta[7] = getAngle(theta_freq[7], theta_min[7], theta_max[7], seconds);
 theta[8] = getAngle(theta_freq[8], theta_min[8], theta_max[8], seconds);
 theta[1] = getAngle(theta_freq[1], theta_min[1], theta_max[1], seconds);
 theta[2] = getAngle(theta_freq[2], theta_min[2], theta_max[2], seconds);
}
 //moving body with right arm,right leg and head
 else if (where_to_rotate == 16) {
 theta[0] = getAngle(theta_freq[0], theta_min[0], theta_max[0], seconds);
 theta[6] = getAngle(theta_freq[6], theta_min[6], theta_max[6], seconds);
 theta[5] = getAngle(theta_freq[5], theta_min[5], theta_max[5], seconds);
 theta[9] = getAngle(theta_freq[9], theta_min[9], theta_max[9], seconds);
 theta[10] = getAngle(theta_freq[10], theta_min[10], theta_max[10], seconds);
 theta[1] = getAngle(theta_freq[1], theta_min[1], theta_max[1], seconds);
 theta[2] = getAngle(theta_freq[2], theta_min[2], theta_max[2], seconds);
 }
 //moving body with left arm,right leg and head
 else if (where_to_rotate == 17) {
 theta[0] = getAngle(theta_freq[0], theta_min[0], theta_max[0], seconds);
 theta[3] = getAngle(theta_freq[3], theta_min[3], theta_max[3], seconds);
 theta[4] = getAngle(theta_freq[4], theta_min[4], theta_max[4], seconds);
 theta[9] = getAngle(theta_freq[9], theta_min[9], theta_max[9], seconds);
 theta[10] = getAngle(theta_freq[10], theta_min[10], theta_max[10], seconds);
 theta[1] = getAngle(theta_freq[1], theta_min[1], theta_max[1], seconds);
 theta[2] = getAngle(theta_freq[2], theta_min[2], theta_max[2], seconds);
 }
 else if (where_to_rotate == 18) {
 theta[0] = getAngle(theta_freq[0], theta_min[0], theta_max[0], seconds);
 theta[6] = getAngle(theta_freq[6], theta_min[6], theta_max[6], seconds);
 theta[5] = getAngle(theta_freq[5], theta_min[5], theta_max[5], seconds);
 theta[7] = getAngle(theta_freq[7], theta_min[7], theta_max[7], seconds);
 theta[8] = getAngle(theta_freq[8], theta_min[8], theta_max[8], seconds);
 theta[1] = getAngle(theta_freq[1], theta_min[1], theta_max[1], seconds);
 theta[2] = getAngle(theta_freq[2], theta_min[2], theta_max[2], seconds);
 }
 glEnable(GL_LIGHT0);
glEnable(GL_LIGHT3);
 glutPostRedisplay();
}
/// My reshape
void reshape(int w, int h) {
 glViewport(0, 0, w, h);
 glMatrixMode(GL_PROJECTION);
 glLoadIdentity();
 if (w <= h)
 glOrtho(-10.0, 10.0, -10.0 * (GLfloat)h / (GLfloat)w,
 10.0 * (GLfloat)h / (GLfloat)w, -10.0, 10.0);
 else
 glOrtho(-10.0 * (GLfloat)w / (GLfloat)h,
 10.0 * (GLfloat)w / (GLfloat)h, -10.0, 10.0, -10.0, 10.0);
 glMatrixMode(GL_MODELVIEW);
 glLoadIdentity();
}
/// init light things
void light_init() {
 /// light 1
 GLfloat light_ambient1[] = { 0.0, 0.0, 0.0, 1.0 };
 GLfloat light_diffuse1[] = { 1.0, 1.0, 1.0, 1.0 };
 GLfloat light_specular1[] = { 1.0, 1.0, 1.0, 1.0 };
 GLfloat light_position1[] = { 10.0, 10.0, 10.0, 0.0 };
 glLightfv(GL_LIGHT0, GL_POSITION, light_position1);
 glLightfv(GL_LIGHT0, GL_AMBIENT, light_ambient1);
 glLightfv(GL_LIGHT0, GL_DIFFUSE, light_diffuse1);
 glLightfv(GL_LIGHT0, GL_SPECULAR, light_specular1);
 ///light 2
 GLfloat light_ambient2[] = { 0.0, 0.0, 0.0, 1.0 };
 GLfloat light_diffuse2[] = { 1.0, 1.0, 1.0, 1.0 };
 GLfloat light_specular2[] = { 1.0, 1.0, 1.0, 1.0 };
 GLfloat light_position2[] = { -10.0, 10.0, 10.0, 0.0 };
glLightfv(GL_LIGHT3, GL_POSITION, light_position2);
 glLightfv(GL_LIGHT3, GL_AMBIENT, light_ambient2);
 glLightfv(GL_LIGHT3, GL_DIFFUSE, light_diffuse2);
 glLightfv(GL_LIGHT3, GL_SPECULAR, light_specular2);
 glShadeModel(GL_SMOOTH);
 glEnable(GL_LIGHTING);
 glDepthFunc(GL_LEQUAL);
 glEnable(GL_DEPTH_TEST);
 glClearColor(0.0, 0.0, 0.0, 1.0);
 glColor3f(0.0, 0.0, 0.0);
}
/// init rotation angles
void rotate_init() {
 /// Setting the min, max and frequency for body parts
 for (int i = 0; i < 11; i++) {
 theta_min[i] = 0.0;
 theta_max[i] = 0.0;
 theta_freq[i] = 0.0;
 }
 /// assigning values for theta
 theta_freq[0] = 0.8; theta_max[0] = 30; theta_min[0] = -30;
 theta_freq[1] = 2.0; theta_max[1] = 15.0; theta_min[1] = -5.0;
 theta_freq[2] = 2.0; theta_max[2] = 20.0; theta_min[2] = 0.0;
 theta_freq[3] = 2.0; theta_max[3] = -100.0; theta_min[3] = 0.0;
 theta_freq[5] = 2.0; theta_max[5] = 100.0; theta_min[5] = 0.0;
 theta_freq[4] = 2.0; theta_max[4] = -35.0; theta_min[4] = -10.0;
 theta_freq[6] = 2.0; theta_max[6] = -35.0; theta_min[6] = -10.0;
 theta_freq[7] = 2.0; theta_max[7] = 200.0; theta_min[7] = 160.0;
 theta_freq[9] = 2.0; theta_max[9] = 160.0; theta_min[9] = 200.0;
 theta_freq[8] = 2.0; theta_max[8] = -35.0; theta_min[8] = -10.0;
 theta_freq[10] = 2.0; theta_max[10] = -35.0; theta_min[10] = -10.0;
}
/// init quadrics style : GLU_FILL
void style_init() {
 /// allocate quadrics with filled drawing style: GLU_FILL
 h = gluNewQuadric(); gluQuadricDrawStyle(h, GLU_FILL);
 t = gluNewQuadric(); gluQuadricDrawStyle(t, GLU_FILL);
 gl = gluNewQuadric(); gluQuadricDrawStyle(gl, GLU_FILL);
 lua = gluNewQuadric(); gluQuadricDrawStyle(lua, GLU_FILL);
 lla = gluNewQuadric(); gluQuadricDrawStyle(lla, GLU_FILL);
 rua = gluNewQuadric(); gluQuadricDrawStyle(rua, GLU_FILL);
 rla = gluNewQuadric(); gluQuadricDrawStyle(rla, GLU_FILL);
 lul = gluNewQuadric(); gluQuadricDrawStyle(lul, GLU_FILL);
 lll = gluNewQuadric(); gluQuadricDrawStyle(lll, GLU_FILL);
 rul = gluNewQuadric(); gluQuadricDrawStyle(rul, GLU_FILL);
 rll = gluNewQuadric(); gluQuadricDrawStyle(rll, GLU_FILL);
}
///********* menu things ***********///
void menu(int id) {
 if (id == 0)
 exit(0);
 if (id == 1)
 where_to_rotate = 1;
 if (id == 2)
 where_to_rotate = 2;
 if (id == 3)
 where_to_rotate = 3;
 if (id == 4)
 where_to_rotate = 4;
 if (id == 5)
 where_to_rotate = 5;
 if (id == 6)
 where_to_rotate = 6;
 if (id == 7)
 where_to_rotate = 7;
if (id == 8)
 where_to_rotate = 8;
 if (id == 9)
 where_to_rotate = 9;
 if (id == 10)
 where_to_rotate = 10;
 if (id == 11)
 where_to_rotate = 11;
 if (id == 12)
 where_to_rotate = 12;
 if (id == 13)
 where_to_rotate = 13;
 if (id ==14)
 where_to_rotate = 14;
 if (id ==15)
 where_to_rotate = 15;
 if (id == 16)
 where_to_rotate = 16;
 if (id == 17)
 where_to_rotate = 17;
 if (id ==18)
 where_to_rotate = 18;
}
void make_menu() {
 glutCreateMenu(menu);
 glutAddMenuEntry("Move right arm", 1);
 glutAddMenuEntry("Move left arm", 2);
 glutAddMenuEntry("Move right leg", 3);
 glutAddMenuEntry("Move left leg", 4);
 glutAddMenuEntry("Move Body", 5);
 glutAddMenuEntry("Move head towards up-down", 6);
 glutAddMenuEntry("Move head towards sideways", 7);
glutAddMenuEntry("Move Body with left arm", 8);
 glutAddMenuEntry("Move Body with right arm", 9);
 glutAddMenuEntry("Move Body with left leg", 10);
 glutAddMenuEntry("Move Body with right leg", 11);
 glutAddMenuEntry("Move Body with head", 12);
 glutAddMenuEntry("Move Body with both arms", 13);
 glutAddMenuEntry("Move Body with both legs", 14);
 glutAddMenuEntry("Move Body with left arm,left leg and head", 15);
 glutAddMenuEntry("Move Body with right arm,right leg and head", 16);
 glutAddMenuEntry("Move Body with left arm,right leg and head", 17);
 glutAddMenuEntry("Move Body with right arm,left leg and head", 18);
 glutAddMenuEntry("exit", 0);
 glutAttachMenu(GLUT_RIGHT_BUTTON);
}
///********* main function ***********///
int main(int argc, char** argv) {
 const wchar_t* path = L"C:\\Users\\rosha\\Downloads\\blinding_lights1.wav";
 glutInit(&argc, argv);
 glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB | GLUT_DEPTH);
 glutInitWindowSize(800, 800);
 glutCreateWindow("dancing robot");
 PlaySound(path, NULL, SND_FILENAME | SND_ASYNC |SND_LOOP);
 //PlaySound("", NULL, SND_ASYNC | SND_FILENAME | SND_LOOP);
 light_init();
 style_init();
 rotate_init(); /// init light, style and rotate
 glutReshapeFunc(reshape);
 glutDisplayFunc(display);
 glutIdleFunc(idle);
 make_menu();
 glutMainLoop();
 return 0;
}