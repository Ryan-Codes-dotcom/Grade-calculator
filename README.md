# index.html
Grade calculator
<script>
    // Generate input fields when number of categories changes
    document.getElementById("numCategories").addEventListener("input", function () {
        const numCategories = parseInt(this.value);
        const categoriesDiv = document.getElementById("categories");
        categoriesDiv.innerHTML = "";
        document.getElementById("result").innerHTML = "";

        if (isNaN(numCategories) || numCategories < 1) {
            return;
        }

        for (let i = 1; i <= numCategories; i++) {
            const categoryDiv = document.createElement("div");
            categoryDiv.classList.add("input-group");

            categoryDiv.innerHTML = `
                <label>Category ${i} Grade (0–100):</label>
                <input type="number" id="grade${i}" min="0" max="100" required>

                <label>Category ${i} Weight (%):</label>
                <input type="number" id="weight${i}" min="0" max="100" required>
            `;

            categoriesDiv.appendChild(categoryDiv);
        }
    });

    function calculateGrade() {
        const numCategories = parseInt(document.getElementById("numCategories").value);
        const resultDiv = document.getElementById("result");

        let totalWeight = 0;
        let weightedSum = 0;

        if (isNaN(numCategories) || numCategories < 1) {
            resultDiv.innerHTML = "<p style='color:red;'>Please enter a valid number of categories.</p>";
            return;
        }

        for (let i = 1; i <= numCategories; i++) {
            const grade = parseFloat(document.getElementById(`grade${i}`).value);
            const weight = parseFloat(document.getElementById(`weight${i}`).value);

            if (
                isNaN(grade) || isNaN(weight) ||
                grade < 0 || grade > 100 ||
                weight < 0 || weight > 100
            ) {
                resultDiv.innerHTML = "<p style='color:red;'>All grades and weights must be between 0 and 100.</p>";
                return;
            }

            weightedSum += grade * (weight / 100);
            totalWeight += weight;
        }

        // Check if weights equal 100%
        if (totalWeight !== 100) {
            resultDiv.innerHTML = "<p style='color:red;'>Total weight must equal 100%.</p>";
            return;
        }

        const finalGrade = weightedSum;
        let letterGrade = "";

        // Conditional statements (important for assignment!)
        if (finalGrade >= 90) {
            letterGrade = "A";
        } else if (finalGrade >= 80) {
            letterGrade = "B";
        } else if (finalGrade >= 70) {
            letterGrade = "C";
        } else if (finalGrade >= 60) {
            letterGrade = "D";
        } else {
            letterGrade = "F";
        }

        resultDiv.innerHTML = `
            <h2>Final Grade: ${finalGrade.toFixed(2)}%</h2>
            <h3>Letter Grade: ${letterGrade}</h3>
        `;
    }
</script>
