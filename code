import java.util.Calendar

data class Subject(
	val title: String, val grade: Int
)

data class Student(
    val name: String, val dateBirth: Int, val subject: List<Subject>
    ){
    val age=Calendar.getInstance().get(Calendar.YEAR)-dateBirth
    val averageGrade: Float
    get() = subject.average{ it.grade.toFloat() }
}

fun <T> Iterable<T>.average(block: (T) -> Float): Float {
    var sum: Double = 0.0
    var count: Int = 0
    for (element in this){
        sum += block(element)
        ++count
    }
    return (sum/count).toFloat()
}

data class University(
    val title: String, val students: MutableList<Student>
    ) {
    val averageGrade: Float
    get() = students.average{ it.averageGrade }

    val courses: Map<Int, List<Student>>
    get() = students
    	.groupBy { it.age }
    	.mapKeys {
        when (it.key) {
        17 -> 1
        18 -> 2
        19 -> 3
        20 -> 4
        21 -> 5
        else -> throw StudentTooYoungException("Error: Student too young")
        }
    }
}

class StudentTooYoungException(message:String): Exception(message)

enum class StudyProgram(val title: String){
    EXSYS("expert system"),
    ENG("english language"),
    MATH("math");

    infix fun withGrade(grade: Int) = Subject(title,grade)
}

typealias StudentsListener = (Student) -> Unit

val students = mutableListOf(
Student("Alexandr",2001,listOf(StudyProgram.EXSYS withGrade 5, StudyProgram.ENG withGrade 4, StudyProgram.MATH withGrade 5)),
Student("Iliya",2001,listOf(StudyProgram.EXSYS withGrade 4, StudyProgram.ENG withGrade 4, StudyProgram.MATH withGrade 3)),
Student("Arthur",2002,listOf(StudyProgram.EXSYS withGrade 5, StudyProgram.ENG withGrade 5, StudyProgram.MATH withGrade 5)),
Student("Dmitry",2000,listOf(StudyProgram.EXSYS withGrade 5, StudyProgram.ENG withGrade 4, StudyProgram.MATH withGrade 5)),
Student("Daniil",2003,listOf(StudyProgram.EXSYS withGrade 3, StudyProgram.ENG withGrade 3, StudyProgram.MATH withGrade 3)),
Student("Pavel",2004,listOf(StudyProgram.EXSYS withGrade 4, StudyProgram.ENG withGrade 4, StudyProgram.MATH withGrade 4)),
Student("Andrew",2000,listOf(StudyProgram.EXSYS withGrade 4, StudyProgram.ENG withGrade 5, StudyProgram.MATH withGrade 5))
)

object DataSource {
	val university : University by lazy {
	University("aaaa", students)
	}

	var onNewStudentListener : StudentsListener ?= null

	fun addStudent(name: String, birthYear: Int, students: MutableList<Student>){
        students.add(Student(name,birthYear,listOf(StudyProgram.EXSYS withGrade 4,StudyProgram.ENG withGrade 5)))
        val addedStudent = students.last()
        onNewStudentListener?.invoke(addedStudent)
    }
}

fun main() {
    DataSource.onNewStudentListener = {
        println("Новый студент: $it " + "Средняя оценка по университету ${DataSource.university.averageGrade}")
    }
    println(DataSource.university.averageGrade)
    println(DataSource.university.courses)
    DataSource.addStudent("Valerii", 2003, students)
}
