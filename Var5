from OpenGL.GL import *
from OpenGL.GLU import *
from OpenGL.GLUT import *
import math

angle = 30.0
x_pos = 20.0
x_direction = -1
shape = 1

rotate_direction = 1

radius = 30.0
yaw = 20.0
pitch = 20.0
last_x, last_y = 400, 300
mouse_down = False


def draw_axes():
    glLineWidth(2.0)
    glBegin(GL_LINES)
    glColor3f(1.0, 0.0, 0.0)
    glVertex3f(-25.0, 0.0, 0.0)
    glVertex3f(25.0, 0.0, 0.0)
    glColor3f(0.0, 1.0, 0.0)
    glVertex3f(0.0, -5.0, 0.0)
    glVertex3f(0.0, 5.0, 0.0)
    glColor3f(0.0, 0.0, 1.0)
    glVertex3f(0.0, 0.0, -5.0)
    glVertex3f(0.0, 0.0, 5.0)
    glEnd()
    glLineWidth(1.0)
    glColor3f(1.0, 1.0, 1.0)


def draw_selected_shape():
    if shape == 1:
        glutSolidCube(1.0)
    elif shape == 2:
        glutSolidTetrahedron()
    elif shape == 3:
        glutSolidOctahedron()


def display():
    global angle, x_pos, yaw, pitch, radius
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)

    glMatrixMode(GL_MODELVIEW)
    glLoadIdentity()

    # Напрямок - сферичні координати перетворюємо в декартові
    camera_x = radius * math.cos(math.radians(pitch)) * math.sin(math.radians(yaw))
    camera_y = radius * math.sin(math.radians(pitch))
    camera_z = radius * math.cos(math.radians(pitch)) * math.cos(math.radians(yaw))

    gluLookAt(camera_x, camera_y, camera_z, 0, 0, 0, 0, 1, 0)

    draw_axes()

    glPushMatrix()
    glTranslatef(x_pos, 2.0, 0.0)

    glRotatef(angle * rotate_direction, 1, 0, 0)

    draw_selected_shape()
    glPopMatrix()

    glutSwapBuffers()


def timer(value):
    global angle, x_pos, x_direction
    angle += 1.0
    x_pos += 0.1 * x_direction
    if x_pos >= 20.0:
        x_direction = -1
    elif x_pos <= -20.0:
        x_direction = 1
    glutPostRedisplay()
    glutTimerFunc(16, timer, 0)


def reshape(w, h):
    if h == 0:
        h = 1
    glViewport(0, 0, w, h)
    glMatrixMode(GL_PROJECTION)
    glLoadIdentity()
    gluPerspective(45, w / h, 1.0, 100.0)
    glMatrixMode(GL_MODELVIEW)


def keyboard(key, x, y):
    global shape
    if key == b'1':
        shape = 1
    elif key == b'2':
        shape = 2
    elif key == b'3':
        shape = 3


def mouse(button, state, x, y):
    global mouse_down, last_x, last_y
    if button == GLUT_LEFT_BUTTON:
        if state == GLUT_DOWN:
            mouse_down = True
            last_x = x
            last_y = y
        elif state == GLUT_UP:
            mouse_down = False


def motion(x, y):
    global yaw, pitch, last_x, last_y
    if mouse_down:
        dx = x - last_x
        dy = y - last_y
        last_x = x
        last_y = y
        yaw += dx * 0.5
        pitch -= dy * 0.5

        pitch = max(-89.0, min(89.0, pitch))
        glutPostRedisplay()


def init():
    glEnable(GL_DEPTH_TEST)
    glClearColor(0.1, 0.1, 0.2, 1.0)


def main():
    global rotate_direction

    user_input = input("enter direction: ")

    if user_input == '1':
        rotate_direction = 1
    elif user_input == '-1':
        rotate_direction = -1
    else:
        print("Invalid input, defaulting to clockwise.")
        rotate_direction = 1

    glutInit()
    glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB | GLUT_DEPTH)
    glutInitWindowSize(800, 600)
    glutCreateWindow(b"Lab4 by Roman Kovalchuk")
    init()
    glutDisplayFunc(display)
    glutReshapeFunc(reshape)
    glutKeyboardFunc(keyboard)
    glutMouseFunc(mouse)
    glutMotionFunc(motion)
    glutTimerFunc(0, timer, 0)
    glutMainLoop()


if __name__ == '__main__':
    main()
