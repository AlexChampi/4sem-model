<script>
import '/src/assets/style.css'
import {complex, index, kron, matrix, multiply, range, sqrt, subset} from "mathjs";

export default {
    name: 'App',

    data: function () {
        return {
            command: "",
            numberOfQubits: 1,
            PauliTargetQubit: 1,
            HadamardTargetQubit: 1,
            CNOTControlQubit: 1,
            CNOTTarhetQubit: 1,
            CCNOTControlQubit1: 1,
            CCNOTControlQubit2: 1,
            CCNotTargetQubit: 1,
            PhaseControlQubit: 1,
            PhaseTargetQubit: 1,
            PhasePhase: 1,
            SwapFirstLine: 1,
            SwapSecondLine: 1,
            error: "",
            arr1: matrix(Array.from({length: 64}, () => {
                return complex(0, 0);
            })),
            // arr1: matrix(Array(64).fill(complex(0, 0))),
            arr2: matrix(Array.from({length: 64}, () => {
                return complex(0, 0);
            })),
        }
    }
    ,
    methods: {
        validateInput() {
            // var cnt = 0;
            // for (let i = 0; i < Math.pow(2, this.numberOfQubits); i++) {
            //     cnt += this.arr1.get([i]).re + this.arr1.get([i]).im;
            // }
            // if (cnt - 0.1 <= 1 && cnt + 0.1 >= 1) {
            //     this.error = "Некорректный ввод!"
            //     return false;
            // }
            // this.error = "";
            return true;
        },
        multiplyOnIdentity(n, m) {
            var res = m;
            var iden = matrix([[complex(1, 0), complex(0, 0)],
                [complex(0, 0), complex(1, 0)]]);
            for (let i = 0; i < n - 1; i++) {
                res = kron(res, iden);
            }
            return res;
        }
        ,
        onClearInput() {
            this.command = "";
        }
        ,
        onApplyInput() {
        }
        ,
        onSetZero() {
            this.arr1 = matrix(Array.from({length: 64}, () => {
                return complex(0, 0);
            }));
            this.arr1.set([0], complex(1, 0));
        }
        ,
        onSetSymbolic() {

        }
        ,
        onReAndIm() {

        }
        ,
        onCopyResultToInput() {
            this.arr1 = this.arr2;
        }
        ,
        onApplySigmaX() {
            if (!this.validateInput()) {
                return;
            }
            var newArr = subset(this.arr1, index(range(0, Math.pow(2, this.numberOfQubits))));
            var m2 = matrix([[complex(0, 0), complex(1, 0)], [complex(1, 0), complex(0, 0)]]);
            var res = matrix([[complex(1, 0), complex(0, 0)],
                [complex(0, 0), complex(1, 0)]]);
            console.log(res.toString());
            if (this.PauliTargetQubit === 1) {
                res = this.multiplyOnIdentity(this.numberOfQubits, m2);
                console.log(res.toString());
            } else {
                res = this.multiplyOnIdentity(this.PauliTargetQubit - 1, res);
                res = kron(res, m2);
                res = this.multiplyOnIdentity(this.numberOfQubits - this.PauliTargetQubit + 1, res);
            }
            var reMul = multiply(res, matrix(newArr));
            console.log(reMul.toString())
            let i = 0;
            reMul.forEach((v) => (this.arr2.set([i++], v)));
            this.command += "_sigX(" + this.PauliTargetQubit + ")"
        }
        ,
        onApplySigmaY() {
            if (!this.validateInput()) {
                return;
            }
            var m2 = matrix([[complex(0, 0), complex(0, -1)], [complex(0, 1), complex(0, 0)]]);
            var newArr = subset(this.arr1, index(range(0, Math.pow(2, this.numberOfQubits))));
            var res = matrix([[complex(1, 0), complex(0, 0)],
                [complex(0, 0), complex(1, 0)]]);
            console.log(res.toString());
            if (this.PauliTargetQubit === 1) {
                res = this.multiplyOnIdentity(this.numberOfQubits, m2);
                console.log(res.toString());
            } else {
                res = this.multiplyOnIdentity(this.PauliTargetQubit - 1, res);
                res = kron(res, m2);
                res = this.multiplyOnIdentity(this.numberOfQubits - this.PauliTargetQubit + 1, res);
            }
            var reMul = multiply(res, matrix(newArr));
            console.log(reMul.toString())
            let i = 0;
            reMul.forEach((v) => (this.arr2.set([i++], v)));
            this.command += "_sigY(" + this.PauliTargetQubit + ")"
        }
        ,
        onApplySigmaZ() {
            if (!this.validateInput()) {
                return;
            }
            var m2 = matrix([[complex(1, 0), complex(0, 0)], [complex(0, 0), complex(-1, 0)]]);
            var newArr = subset(this.arr1, index(range(0, Math.pow(2, this.numberOfQubits))));
            var res = matrix([[complex(1, 0), complex(0, 0)],
                [complex(0, 0), complex(1, 0)]]);
            console.log(res.toString());
            if (this.PauliTargetQubit === 1) {
                res = this.multiplyOnIdentity(this.numberOfQubits, m2);
                console.log(res.toString());
            } else {
                res = this.multiplyOnIdentity(this.PauliTargetQubit - 1, res);
                res = kron(res, m2);
                res = this.multiplyOnIdentity(this.numberOfQubits - this.PauliTargetQubit + 1, res);
            }
            var reMul = multiply(res, matrix(newArr));
            console.log(reMul.toString())
            let i = 0;
            reMul.forEach((v) => (this.arr2.set([i++], v)));
            this.command += "_sigX(" + this.PauliTargetQubit + ")"
        }
        ,
        onHadamardApply() {
            if (!this.validateInput()) {
                return;
            }
            var m2 = matrix([[sqrt(2) * complex(1, 0) / 2, sqrt(2) * complex(1, 0) / 2],
                [sqrt(2) * complex(1, 0) / 2, -sqrt(2) * complex(1, 0) / 2]]);
            var newArr = subset(this.arr1, index(range(0, Math.pow(2, this.numberOfQubits))));
            var res = matrix([[complex(1, 0), complex(0, 0)],
                [complex(0, 0), complex(1, 0)]]);
            console.log(res.toString());
            if (this.HadamardTargetQubit === 1) {
                res = this.multiplyOnIdentity(this.numberOfQubits, m2);
                console.log(res.toString());
            } else {
                res = this.multiplyOnIdentity(this.HadamardTargetQubit - 1, res);
                res = kron(res, m2);
                res = this.multiplyOnIdentity(this.numberOfQubits - this.HadamardTargetQubit + 1, res);
            }
            var reMul = multiply(res, matrix(newArr));
            console.log(reMul.toString())
            let i = 0;
            reMul.forEach((v) => (this.arr2.set([i++], v)));
            this.command += "_H(" + this.HadamardTargetQubit + ")"
        }
        ,
        onWalshApply() {
            if (!this.validateInput()) {
                return;
            }
            var m2 = matrix([[sqrt(2) * complex(1, 0) / 2, sqrt(2) * complex(1, 0) / 2],
                [sqrt(2) * complex(1, 0) / 2, -sqrt(2) * complex(1, 0) / 2]]);
            var newArr = subset(this.arr1, index(range(0, Math.pow(2, this.numberOfQubits))));
            var res = m2;
            console.log(res.toString());
            for (let i = 0; i < this.numberOfQubits - 1; i++) {
                res = kron(res, m2);
            }
            var reMul = multiply(res, matrix(newArr));
            console.log(reMul.toString())
            let i = 0;
            reMul.forEach((v) => (this.arr2.set([i++], v)));
            this.command += "_WH(" + this.HadamardTargetQubit + ")"
        }
        ,
        onCNOTApply() {
            if (!this.validateInput()) {
                return;
            }
            var m4 = matrix([
                [complex(1, 0), complex(0, 0), complex(0, 0), complex(0, 0)],
                [complex(0, 0), complex(1, 0), complex(0, 0), complex(0, 0)],
                [complex(0, 0), complex(0, 0), complex(0, 0), complex(1, 0)],
                [complex(0, 0), complex(0, 0), complex(1, 0), complex(0, 0)]
            ]);
            var newArr = subset(this.arr1, index(range(0, Math.pow(2, this.numberOfQubits))));
            var res = matrix([
                [complex(1, 0), complex(0, 0), complex(0, 0), complex(0, 0)],
                [complex(0, 0), complex(1, 0), complex(0, 0), complex(0, 0)],
                [complex(0, 0), complex(0, 0), complex(0, 0), complex(1, 0)],
                [complex(0, 0), complex(0, 0), complex(1, 0), complex(0, 0)]
            ]);
            console.log(res.toString());
            if (this.CNOTControlQubit === 1) {
                res = this.multiplyOnIdentity(this.numberOfQubits - 1, m4);
                console.log(res.toString());
            } else {
                res = this.multiplyOnIdentity(this.CNOTControlQubit - 1, res);
                res = kron(res, m4);
                res = this.multiplyOnIdentity(this.numberOfQubits - this.CNOTTarhetQubit, res);
            }
            var reMul = multiply(res, matrix(newArr));
            console.log(reMul.toString())
            let i = 0;
            reMul.forEach((v) => (this.arr2.set([i++], v)));
            this.command += "_CNOT(" + this.CNOTControlQubit + ")"
        }
        ,
        onCCNOTApply() {
            if (!this.validateInput()) {
                return;
            }
            var t0 = complex(0, 0);
            var t1 = complex(1, 0);
            // var m4 = matrix([
            //     [t1, t0, t0, t0, t0, t0, t0, t0],
            //     [t0, t1, t0, t0, t0, t0, t0, t0],
            //     [t0, t0, t1, t0, t0, t0, t0, t0],
            //     [t0, t0, t0, t1, t0, t0, t0, t0],
            //     [t0, t0, t0, t0, t1, t0, t0, t0],
            //     [t0, t0, t0, t0, t0, t1, t0, t0],
            //     [t0, t0, t0, t0, t0, t0, t0, t1],
            //     [t0, t0, t0, t0, t0, t0, t1, t0]
            // ]);
            var newArr = subset(this.arr1, index(range(0, Math.pow(2, this.numberOfQubits))));
            var res = matrix([
                [t1, t0, t0, t0, t0, t0, t0, t0],
                [t0, t1, t0, t0, t0, t0, t0, t0],
                [t0, t0, t1, t0, t0, t0, t0, t0],
                [t0, t0, t0, t1, t0, t0, t0, t0],
                [t0, t0, t0, t0, t1, t0, t0, t0],
                [t0, t0, t0, t0, t0, t1, t0, t0],
                [t0, t0, t0, t0, t0, t0, t0, t1],
                [t0, t0, t0, t0, t0, t0, t1, t0]
            ]);
            console.log(res.toString());
            // if (this.CCNOTControlQubit1 === 1) {
            //     res = this.multiplyOnIdentity(this.numberOfQubits - 2, m4);
            //     console.log(res.toString());
            // } else {
            //     res = this.multiplyOnIdentity(this.CCNOTControlQubit1 - 1, res);
            //     res = kron(res, m4);
            //     res = this.multiplyOnIdentity(this.numberOfQubits - this.CCNotTargetQubit - 1, res);
            // }
            var reMul = multiply(res, matrix(newArr));
            console.log(reMul.toString())
            let i = 0;
            reMul.forEach((v) => (this.arr2.set([i++], v)));
            this.command += "_CCNOT(" + this.CCNOTControlQubit1 + ")"
        }
        ,
        onPhaseApply() {

        }
        ,
        onSwapApply() {
            if (!this.validateInput()) {
                return;
            }
            var m4 = matrix([
                [complex(1, 0), complex(0, 0), complex(0, 0), complex(0, 0)],
                [complex(0, 0), complex(0, 0), complex(1, 0), complex(0, 0)],
                [complex(0, 0), complex(1, 0), complex(0, 0), complex(0, 0)],
                [complex(0, 0), complex(0, 0), complex(0, 0), complex(1, 0)]
            ]);
            var newArr = subset(this.arr1, index(range(0, Math.pow(2, this.numberOfQubits))));
            var res =matrix([
                [complex(1, 0), complex(0, 0), complex(0, 0), complex(0, 0)],
                [complex(0, 0), complex(0, 0), complex(1, 0), complex(0, 0)],
                [complex(0, 0), complex(1, 0), complex(0, 0), complex(0, 0)],
                [complex(0, 0), complex(0, 0), complex(0, 0), complex(1, 0)]
            ]);
            console.log(res.toString());
            if (this.CNOTControlQubit === 1) {
                res = this.multiplyOnIdentity(this.numberOfQubits - 1, m4);
                console.log(res.toString());
            } else {
                res = this.multiplyOnIdentity(this.CNOTControlQubit - 1, res);
                res = kron(res, m4);
                res = this.multiplyOnIdentity(this.numberOfQubits - this.CNOTTarhetQubit, res);
            }
            var reMul = multiply(res, matrix(newArr));
            console.log(reMul.toString())
            let i = 0;
            reMul.forEach((v) => (this.arr2.set([i++], v)));
            this.command += "_CNOT(" + this.CNOTControlQubit + ")"
        }
    }
}
</script>


<template>
    <div id="appl">
        <div class="main">
            <div class="error">{{ error }}</div>
            <div class="body">
                <div class="column">
                    <div class="header">Input</div>
                    <div class="table">
                        <table id="left">
                            <tr>
                                <th></th>
                                <th>Re</th>
                                <th>Im</th>
                            </tr>
                            <tr v-for="i in Math.pow(2, numberOfQubits)" :key="i">
                                <td>{{ i }}</td>
                                <td><input v-model="arr1.get([i - 1]).re" type="number"/></td>
                                <td><input v-model="arr1.get([i - 1]).im" type="number"/></td>
                            </tr>
                        </table>
                    </div>
                </div>
                <div class="column">
                    <div class="header">Output</div>
                    <div class="table">
                        <table id="right">
                            <tr>
                                <th></th>
                                <th>Re</th>
                                <th>Im</th>
                            </tr>
                            <tr v-for="i in  Math.pow(2, numberOfQubits)" :key="i">
                                <td>{{ i }}</td>
                                <td>{{ arr2.get([i - 1]).re }}</td>
                                <td>{{ arr2.get([i - 1]).im }}i</td>
                            </tr>
                        </table>
                    </div>
                </div>
            </div>
            <div class="footer">
                <div class="input-field">
                    <input v-model="command" type="text"/>
                    <div class="input-buttons">
                        <button @click.prevent="onClearInput">clear</button>
                        <button @click.prevent="onApplyInput">apply</button>
                    </div>
                </div>
                <div class="setters">
                    <div class="g-buttons">
                        <div class="qbit-numbers">
                            Numbers of qubits
                            <input v-model="numberOfQubits" type="number"/>
                        </div>
                        <button @click.prevent="onSetZero">Set zero</button>
                        <button>Set symbolic</button>
                        <button>Set symbolic by Re and Im</button>
                    </div>
                    <div class="g-buttons">
                        <button @click.prevent="onCopyResultToInput">Copy result to input</button>
                        <button>Set zero</button>
                        <button>Set symbolic</button>
                        <button>Set symbolic by Re and Im</button>
                    </div>
                </div>
            </div>
        </div>
        <div class="sidebar">
            <div class="body">
                <div class="header">
                    Gates
                </div>
                <div class="block">
                    <div class="name">
                        Pauli Martices
                    </div>
                    <div class="s-buttons">
                        <div class="int-input">
                            Target qubit
                            <input v-model="PauliTargetQubit" type="number"/>
                        </div>
                        <button @click.prevent="onApplySigmaX">Apply SigmaX</button>
                        <button @click.prevent="onApplySigmaY">Apply SigmaY</button>
                        <button @click.prevent="onApplySigmaZ">Apply SigmaZ</button>
                    </div>
                </div>
                <div class="block">
                    <div class="name">
                        Hadamard
                    </div>
                    <div class="s-buttons">
                        <div class="int-input">
                            Target qubit
                            <input v-model="HadamardTargetQubit" type="number"/>
                        </div>
                        <button @click.prevent="onHadamardApply">Apply</button>
                    </div>
                </div>
                <div class="block">
                    <div class="name">
                        Walsh-Hadamard
                    </div>
                    <div class="s-buttons">
                        <button @click.prevent="onWalshApply">Apply</button>
                    </div>
                </div>
                <div class="block">
                    <div class="name">
                        Controlled NOT
                    </div>
                    <div class="s-buttons">
                        <div class="int-input">
                            Control qubit
                            <input v-model="CNOTControlQubit" type="number"/>
                        </div>
                        <div class="int-input">
                            Target qubit
                            <input v-model="CNOTTarhetQubit" type="number"/>
                        </div>
                        <button @click.prevent="onCNOTApply">Apply</button>
                    </div>
                </div>
                <div class="block">
                    <div class="name">
                        Controlled Controlled NOT
                    </div>
                    <div class="s-buttons">
                        <div class="int-input">
                            Control qubit
                            <input v-model="CCNOTControlQubit1" type="number"/>
                        </div>
                        <div class="int-input">
                            Control qubit
                            <input v-model="CCNOTControlQubit2" type="number"/>
                        </div>
                        <div class="int-input">
                            Target qubit
                            <input v-model="CCNotTargetQubit" type="number"/>
                        </div>
                        <button @click.prevent="onCCNOTApply">Apply</button>
                    </div>
                </div>
                <div class="block">
                    <div class="name">
                        Phase
                    </div>
                    <div class="s-buttons">
                        <div class="int-input">
                            Control qubit
                            <input v-model="PhaseControlQubit" type="number"/>
                        </div>
                        <div class="int-input">
                            Target qubit
                            <input v-model="PhaseTargetQubit" type="number"/>
                        </div>
                        <div class="int-input">
                            Phase
                            <input v-model="PhasePhase" type="number"/>
                        </div>
                        <button @click.prevent="onPhaseApply">Apply</button>
                    </div>
                </div>
                <div class="block">
                    <div class="name">
                        Swap
                    </div>
                    <div class="s-buttons">
                        <div class="int-input">
                            First line
                            <input v-model="SwapFirstLine" type="number"/>
                        </div>
                        <div class="int-input">
                            Second line
                            <input v-model="SwapSecondLine" type="number"/>
                        </div>
                        <button @click.prevent="onSwapApply">Apply</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style>
</style>
