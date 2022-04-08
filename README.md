# Лабораторная работа №1 по дисциплине "Инженерная и компьютерная графика"
***
## 1) Установка необходимых библиотек
     freeglut
     glew
     glm
## 2) Создание окна
```c++
glutInit(&argc, argv); //здесь мы инициализируем GLUT
glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGBA);//так настраиваются некоторые опции GLUT.
//GLUT_DOUBLE включает двойную буферизацию
glutInitWindowSize(1024, 768); //задание размера окна (ширина, высота)
glutInitWindowPosition(100, 100);//задание начальной позиции окна
glutCreateWindow("Tutorial 01");// создаём окно с названием "Tutorial 01"
glutDisplayFunc(RenderSceneCB);//отрисовываем 1 кадр
glClearColor(0.0f, 0.0f, 1.0f, 0.0f);//Устанавливаем цвет, который будет использован во время очистки буфера кадра. 
//Цвет имеет 4 канала (красный, зелёный, синий, альфа-канал) и принимает значения от 0.0 и до 1.0
glutMainLoop();//этот вызов передаёт контроль GLUT'у, который теперь начнёт свой собственный цикл
```
```c++
GLenum res = glewInit();
	if (res != GLEW_OK)
	{
		fprintf(stderr, "Error: '%s'\n", glewGetErrorString(res));
		return 1;
	} // инициализируем GLEW и проверяем на ошибки.
```
```c++
 void RenderSceneCB()
 {
    glClear(GL_COLOR_BUFFER_BIT); //очистка буфера кадра
    glutSwapBuffers(); //функция просит GLUT поменять фоновый буфер и буфер кадра местами
}
 ```
## 3) Рисование точки
```c++
GLuint VBO; //глобальная переменная, хранящая указатель на буфер вершин
```
Добавляем в main():
```c++
glm::vec3 Vertices[1]; //создание вектора вершин (векторов из glm)
Vertices[0] = glm::vec3(0.0f, 0.0f, 0.0f); // создание вектора с координатами x=0, y=0, z=0. Так задается точка в середине экрана.
glGenBuffers(1, &VBO); //генерация объектов переменных типов
glBindBuffer(GL_ARRAY_BUFFER, VBO);//буфер будет хранить массив вершин
glBufferData(GL_ARRAY_BUFFER, sizeof(Vertices), Vertices, GL_STATIC_DRAW);//наполняем объект данными
```
Добавляем в RenderSceneCB():
```c++
glEnableVertexAttribArray(0); //координаты вершин, используемые в буфере, рассматриваются как атрибут вершины с индексом 0
glBindBuffer(GL_ARRAY_BUFFER, VBO); //обратно привязываем наш буфер, приготавливая его для отрисовки
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 0, 0); // вызов говорит конвейеру как воспринимать данные внутри буфера
glDrawArrays(GL_TRIANGLES, 0, 3);//вызвали функцию для отрисовки
glDisableVertexAttribArray(0);//отключаем каждый атрибут вершины
```
## 4) Рисование трегольника
```c++
glm::vec3 Vertices[3]; //Мы увеличили массив, что бы он мог содержать 3 вершины
Vertices[0] = glm::vec3(0.0f, 1.0f, 0.0f);
Vertices[1] = glm::vec3(-1.0f, 0.0f, 0.0f);
Vertices[2] = glm::vec3(0.0f, -1.0f, 0.0f); // изменили координаты треугольника так, чтобы треугольник был повернут в левую сторону
glDrawArrays(GL_TRIANGLES, 0, 3);//Мы рисуем треугольники вместо точек и принимаем 3 вершины вместо 1
```
