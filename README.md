
#include<iostream>
#include<fstream>
#include<iomanip>
#include<string>
using namespace std;
#define MAXTASK 20
//defining task as struct
struct Task
{
	string name;
	double dueDay, dueMonth, dueYear;
	bool isCompleted;
	int priority;
};

int getData(fstream& file, struct Task task[])
{
	int i;
	string text;

	//for loop till max is reached
	for (i = 0; i < MAXTASK && !file.eof(); i++)
	{
		getline(file, task[i].name, '\t');
		file >> task[i].dueDay >> task[i].dueMonth >> task[i].dueYear >> task[i].priority;
		getline(file, text, '\n');
		task[i].isCompleted = 0;
	}
	return i;
}
//view task function
void viewTasks(struct Task tasks[], int records)
{
	//Output
	cout << "No " << left << setw(30) << "Name of Task" << setw(10) << "Due Day" << setw(10) << "Due Month" << setw(10) << "Due Year" << setw(10) << "Priority" << setw(10) << "Completion Status" << endl;
	cout << endl;
	cout << "***********************************************" << endl;

	for (int i = 0; i < records; i++)
	{
		cout << i + 1 << "  " << left << setw(30) << tasks[i].name << setw(10) << tasks[i].dueDay << setw(10) << tasks[i].dueMonth << setw(10) << tasks[i].dueYear << setw(10) << tasks[i].priority << setw(10);
		if (tasks[i].isCompleted == 1)
		{
			cout << "Completed";
		}
		else
			cout << "Not Completed";
		cout << endl;
	}
	cout << "*****************************************" << endl;
	return;
}

//Adding a new task
void addTask(struct Task task[], int records)
{
	cout << "Please enter the details of the task to be added to the To-Do List: " << endl;
	cout << "Name of the task: " << endl;
	getline(cin, task[records - 1].name);
	cout << "Due Day of the Task: " << endl;
	cin >> task[records - 1].dueDay;
	cout << "Due Month of the Task: " << endl;
	cin >> task[records - 1].dueMonth;
	cout << "Due Year of the Task: " << endl;
	cin >> task[records - 1].dueYear;
	cout << "Priority of the Task: " << endl;
	cin >> task[records - 1].priority;
	task[records - 1].isCompleted = 0;
	cout << "The New Task has been Successfully Added" << endl;
	cout << "Press Any Key to Continue..." << endl;
	cin.ignore(1);
	getchar();
	return;
}
//Mark task as completed
void completeTask(struct Task tasks[], int records)
{
	viewTasks(tasks, records);

	int task;
	do
	{
		cout << "Enter the task number that is to be marked completed: " << endl;
		cin >> task;
		if (task<1 || task>records);
		cout << "Invalid Task Number. Must be 1-" << records;
	}

	while (task<1 || task>records);

	if (tasks[task - 1].isCompleted == 0)
	{
		tasks[task - 1].isCompleted = 1;
		cout << "The Task " << task << " has been marked completed" << endl;
	}
	else
		cout << "Task has already been completed" << endl;
	cout << "Press Any Key to Continue..." << endl;

	cin.ignore(1);
	getchar();
	return;
}
//removing task option
int removeTask(struct Task tasks[], int records)
{
	viewTasks(tasks, records);

	int task;
	do
	{
		cout << "Enter the task number that is to be Removed: " << endl;
		cin >> task;
		if (task<1 || task>records)
			cout << "Invalid Task Number. Must be 1-" << records << endl;

	}

	while (task<1 || task>records);

	for (int i = task - 1; i < records - 1; i++)
	{
		tasks[i].name = tasks[i + 1].name;
		tasks[i].dueDay = tasks[i + 1].dueDay;
		tasks[i].dueMonth = tasks[i + 1].dueMonth;
		tasks[i].dueYear = tasks[i + 1].dueYear;
		tasks[i].priority = tasks[i + 1].priority;
		tasks[i].isCompleted = tasks[i + 1].isCompleted;
	}
	records--;
	cout << endl;
	cout << "The Task " << task << "has been successfully Removed" << endl;
	cin.ignore(1);
	cout << "Press Any Key to Continue..." << endl;
	getchar();
	return records;

}

//sorting task
void sortTasks(struct Task tasks[], int records)
{
	for (int i = 0; i < records - 1; i++)
		for (int j = 0; j < records - 1 - i; j++)
			if ((tasks[j].dueYear + tasks[j].dueMonth + tasks[j].dueDay) < (tasks[j + 1].dueYear + tasks[j + 1].dueMonth + tasks[j + 1].dueDay))
				swap(tasks[j], tasks[j + 1]);
	cout << endl;
	cout << "Sorted the To-Do List" << endl;
	cout << "Press Any Key to Continue..." << endl;
	getchar();
	return;
}

void taskSummary(struct Task tasks[], int records)
{
	cout << "Summary of Rudy's To-Do List: " << endl;
	cout << "Total Tasks: " << records << endl;
	int completed = 0;
	for (int i = 0; i < records; i++)
		if (tasks[i].isCompleted == 1)
			completed++;
	cout << "Percentage completed: " << ((float)completed / (float)records) * 100 << "%" << endl;
	cout << "Press Any Key to continue...";
	getchar();
	return;

}
//main function
int main()
{
	struct Task tasks[MAXTASK];
	fstream file;
	file.open("tasks.txt", ios::in);

	int i;

	if (!file)
	{
		cout << "The file tasks.txt does not exists" << endl;
		cout << "Please create a tasks.txt file with the given information" << endl;
		cout << "Exit the Program" << endl;
		return 0;
	}

	int records = getData(file, tasks);
	cout << records << " tasks has been added to the To-Do List from tasks.txt file" << endl;

	//Lets user choose
	int choice;
	do
	{
		cout << endl;
		cout << "****************************************************************" << endl;
		cout << "*		       CHOOSE AN ACTION TO PERFORM					    *" << endl;
		cout << "****************************************************************" << endl;
		cout << "* 1. Veiw the task in the To-Do List                           *" << endl;
		cout << "* 2. Add a New Task to the To-Do List                          *" << endl;
		cout << "* 3. Mark a Task as Completed in the To-Do List                *" << endl;
		cout << "* 4. Remove a Task as from the To-Do List                      *" << endl;
		cout << "* 5. Sort Tasks in the To-Do List on Due Date                  *" << endl;
		cout << "* 6. Display Summary of the Tasks in the To-Do List            *" << endl;
		cout << "* 7. Exit from Program                                         *" << endl;
		cout << "****************************************************************" << endl;

		do
		{
			cout << "Please choose an action to perform (1-7): " << endl;
			cin >> choice;
			cin.ignore(1);
			if (choice < 1 || choice >6)
				cout << "Invalid action. Please enter a number from 1-7" << endl;
		}

		while (choice < 1 || choice>7);

		switch (choice)
		{
		case 1:
			viewTasks(tasks, records);
			cout << "Press Any Key to continue..." << endl;
			getchar();
			break;

		case 2:
			if (records < MAXTASK)
			{
				records++;
				addTask(tasks, records);
			}
			else
				cout << "No more space in the To-Do List" << endl;
			break;

		case 3:
			completeTask(tasks, records);
			break;

		case 4:
			records = removeTask(tasks, records);
			break;

		case 5:
			sortTasks(tasks, records);
			break;

		case 6:
			taskSummary(tasks, records);
			break;

		case 7:
			cout << "Have a Good Day!" << endl;
			break;
		}
	}
	while (choice != 7);
	return 0;
}

