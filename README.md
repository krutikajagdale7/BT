// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract StudentData {
    // Define a structure to represent a Student
    struct Student {
        string name;
        uint age;
        string course;
    }

    // Dynamic array to store multiple students 
    Student[] public students;

    // Event emitted when a new student is added
    event StudentAdded(string name, uint age, string course);

    // Event to log fallback and receive function invocations
    event FallbackCalled(address sender, uint value);
    event ReceiveCalled(address sender, uint value);

    // Modifier to ensure a valid age (example of input validation)
    modifier validAge(uint _age) {
        require(_age > 0, "Age must be positive");
        _;
    }

    // Function to add a new student
    function addStudent(string memory _name, uint _age, string memory _course) public validAge(_age) {
        students.push(Student(_name, _age, _course));
        emit StudentAdded(_name, _age, _course);
    }

    // Function to retrieve the total number of students
    function getTotalStudents() public view returns (uint) {
        return students.length;
    }

    // Fallback function triggered when someone sends Ether or calls a function that doesn't exist
    fallback() external payable {
        // Logic for fallback: Log an event and handle Ether
        emit FallbackCalled(msg.sender, msg.value);    
    }
    // Receive function triggered when Ether is sent to the contract with no data
    receive() external payable {
        // Logic for receiving Ether: Log an event
        emit ReceiveCalled(msg.sender, msg.value);
    }
    // Function to retrieve a student's information based on their index in the array
    function getStudent(uint index) public view returns (string memory name, uint age, string memory course) {
        require(index < students.length, "Invalid index");
        Student memory student = students[index];
        return (student.name, student.age, student.course);
    }
}      
