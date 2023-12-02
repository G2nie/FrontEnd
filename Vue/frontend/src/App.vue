<template>
  <div>
    <h1>Front-end 과제</h1>
    <h2>1학년</h2>
    <button @click="addRow">추가</button>
    <button @click="deleteRow">삭제</button>
    <button @click="calculate">저장</button>
    <table>
      <thead>
        <tr>
          <th>이수</th>
          <th>필수</th>
          <th>과목명</th>
          <th>학점</th>
          <th>출석점수</th>
          <th>과제점수</th>
          <th>중간고사</th>
          <th>기말고사</th>
          <th>총점</th>
          <th>평균</th>
          <th>성적</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, rowIndex) in tableData" :key="rowIndex">
          <td>
            <select v-model="row.isu">
              <option value="교양">교양</option>
              <option value="전공">전공</option>
            </select>
          </td>
          <td>
            <select v-model="row.required">
              <option value="필수">필수</option>
              <option value="선택">선택</option>
            </select>
          </td>
          <td><input v-model="row.subject" type=text></td>
          <td><input v-model="row.credit" type=number></td>
          <td contenteditable="true" @input="updateCellValue(rowIndex, 'attendance', $event)">{{ row.attendance }}</td>
          <td contenteditable="true" @input="updateCellValue(rowIndex, 'assignment', $event)">{{ row.assignment }}</td>
          <td contenteditable="true" @input="updateCellValue(rowIndex, 'midterm', $event)">{{ row.midterm }}</td>
          <td contenteditable="true" @input="updateCellValue(rowIndex, 'final', $event)">{{ row.final }}</td>
          <td>{{ row.total }}</td>
          <td>{{ row.average }}</td>
          <td>{{ row.grade }}</td>
        </tr>
      </tbody>
      <tfoot>
        <tr>
          <td colspan="3">합계</td>
          <td>{{ totalCredits }}</td>
          <td>{{ totalAttendance }}</td>
          <td>{{ totalAssignment }}</td>
          <td>{{ totalMidterm }}</td>
          <td>{{ totalFinal }}</td>
          <td>{{ totalTotal }}</td>
          <td>{{ totalAverage }}</td>
          <td>{{ totalGrade }}</td>
        </tr>
      </tfoot>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [
        {
          isu: "교양",
          required: "필수",
          subject: "",
          credit: "",
          attendance: 0,
          assignment: 0,
          midterm: 0,
          final: 0,
          total: 0,
          average: 0,
          grade: ""
        }
      ],
    };
  },
  computed: {
    totalCredits() {
      return this.tableData.reduce((sum, row) => sum + parseFloat(row.credit), 0);
    },
    totalAttendance() {
      return this.tableData.reduce((sum, row) => sum + parseFloat(row.attendance), 0);
    },
    totalAssignment() {
      return this.tableData.reduce((sum, row) => sum + parseFloat(row.assignment), 0);
    },
    totalMidterm() {
      return this.tableData.reduce((sum, row) => sum + parseFloat(row.midterm), 0);
    },
    totalFinal() {
      return this.tableData.reduce((sum, row) => sum + parseFloat(row.final), 0);
    },
    totalTotal() {
      return this.tableData.reduce((sum, row) => sum + parseFloat(row.total), 0);
    },
    totalAverage() {
      const totalCredits = this.totalCredits;
      return totalCredits > 0 ? (this.totalTotal / totalCredits).toFixed(2) : 0;
    },
    totalGrade() {
      const totalAverage = this.totalAverage;
      if (totalAverage >= 90) return "A+";
      else if (totalAverage >= 80) return "A";
      else if (totalAverage >= 70) return "B";
      else if (totalAverage >= 60) return "C";
      else if (totalAverage >= 50) return "D";
      else return "F";
    }
  },
  methods: {
    addRow() {
      this.tableData.push({
        isu: "교양",
        required: "필수",
        subject: "",
        credit: "",
        attendance: 0,
        assignment: 0,
        midterm: 0,
        final: 0,
        total: 0,
        average: 0,
        grade: ""
      });
    },
    deleteRow(rowIndex) {
      this.tableData.splice(rowIndex, 1);
      this.calculate();
    },
    updateCellValue(rowIndex, field, event) {
      const value = parseFloat(event.target.textContent);
      if (!isNaN(value)) {
        this.tableData[rowIndex][field] = value;
        this.calculate();
      }
    },
    calculate() {
      for (const row of this.tableData) {
        row.total = row.attendance + row.assignment + row.midterm + row.final;
        row.average = (row.total / 4).toFixed(2);
        if (row.average >= 90) row.grade = "A+";
          else if (row.average >= 80) row.grade = "A";
          else if (row.average >= 70) row.grade = "B";
          else if (row.average >= 60) row.grade = "C";
          else if (row.average >= 50) row.grade = "D";
          else row.grade = "F";
      }
    }
  }
};
</script>
