import { useState, useRef } from "react";

const PROBLEM_BANK = {
  beginner: [
    { id: 1, question: "15 + 28 を計算せよ。", points: 5, topic: "加減算" },
    { id: 2, question: "72 ÷ 8 を計算せよ。", points: 5, topic: "乗除算" },
    { id: 3, question: "1/2 + 1/3 を計算し、既約分数で答えよ。", points: 5, topic: "分数" },
    { id: 4, question: "0.3 × 0.7 を計算せよ。", points: 5, topic: "小数" },
    { id: 5, question: "2² + 3² を計算せよ。", points: 5, topic: "べき乗" },
  ],
  intermediate: [
    { id: 1, question: "3x + 7 = 22 のとき、x の値を求めよ。", points: 10, topic: "一次方程式" },
    { id: 2, question: "√144 を計算せよ。", points: 10, topic: "平方根" },
    { id: 3, question: "二次方程式 x² - 5x + 6 = 0 を解け。", points: 20, topic: "二次方程式" },
    { id: 4, question: "1/3 + 1/4 を計算し、既約分数で答えよ。", points: 10, topic: "分数" },
    { id: 5, question: "sin(30°) の値を答えよ。", points: 10, topic: "三角関数" },
  ],
  advanced: [
    { id: 1, question: "∫(2x + 3)dx を不定積分せよ。", points: 15, topic: "積分" },
    { id: 2, question: "行列 A = [[1,2],[3,4]] の行列式 |A| を求めよ。", points: 15, topic: "行列" },
    { id: 3, question: "lim(x→0) (sin x / x) の値を求めよ。", points: 20, topic: "極限" },
    { id: 4, question: "等差数列 2, 5, 8, ... の第10項を求めよ。", points: 15, topic: "数列" },
    { id: 5, question: "log₂(32) の値を求めよ。", points: 15, topic: "対数" },
  ],
};

const LEVEL_META = {
  beginner:     { label: "入門", emoji: "🌱", color: "#22c55e", bg: "#0a1f12", border: "#1a3a24" },
  intermediate: { label: "標準", emoji: "📘", color: "#3b82f6", bg: "#0f1e35", border: "#1a2e50" },
  advanced:     { label: "応用", emoji: "🔥", color: "#f59e0b", bg: "#1f1400", border: "#3a2800" },
};

const STATUS_COLOR = { correct: "#22c55e", partial: "#f59e0b", incorrect: "#ef4444" };
const STATUS_LABEL = { correct: "正解", partial: "部分点", incorrect: "不正解" };

function getGrade(pct) {
  if (pct >= 90) return { label: "A", color: "#22c55e" };
  if (pct >= 75) return { label: "B", color: "#3b82f6" };
  if (pct >= 60) return { label: "C", color: "#f59e0b" };
  if (pct >= 40) return { label: "D", color: "#f97316" };
  return { label: "F", color: "#ef4444" };
}

async function callClaude(prompt) {
  const res = await fetch("https://api.anthropic.com/v1/messages", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      model: "claude-sonnet-4-20250514",
      max_tokens: 1500,
      messages: [{ role: "user", content: prompt }],
    }),
  });
  const data = await res.json();
  const text = data.content.map((i) => i.text || "").join("");
  return text.replace(/```json[\s\S]*?```|```/g, "").trim();
}

export default function MathGrader() {
  const [screen, setScreen] = useState("home");
  const [level, setLevel] = useState("intermediate");
  const [studentName, setStudentName] = useState("");
  const [problems, setProblems] = useState(PROBLEM_BANK.intermediate);
  const [answers, setAnswers] = useState({});
  const [loading, setLoading] = useState(false);
  const [loadingMsg, setLoadingMsg] = useState("");
  const [results, setResults] = useState(null);
  const [history, setHistory] = useState([]);
  const [retryProblems, setRetryProblems] = useState([]);
  const [retryAnswers, setRetryAnswers] = useState({});
  const [retryResults, setRetryResults] = useState(null);
  const [similarProblems, setSimilarProblems] = useState([]);
  const [similarAnswers, setSimilarAnswers] = useState({});
  const [similarResults, setSimilarResults] = useState(null);
  const [historyDetail, setHistoryDetail] = useState(null);
  const topRef = useRef(null);

  const scrollTop = () => setTimeout(() => topRef.current?.scrollIntoView({ behavior: "smooth" }), 80);

  const gradeProblems = async (probs, ans, msg) => {
    setLoading(true);
    setLoadingMsg(msg);
    const text = probs.map((p, i) => `問${i + 1}（id:${p.id}, ${p.points}点）: ${p.question}\n生徒の解答: ${ans[p.id] || "（未回答）"}`).join("\n\n");
    const prompt = `あなたは数学の採点者です。以下を採点してください。\n\n${text}\n\n以下のJSON形式のみで返してください（説明文・マークダウン記号なし）:\n{"results":[{"id":問題のid(数値),"status":"correct"または"partial"または"incorrect","score":得点(数値),"maxScore":満点(数値),"correctAnswer":"正解","feedback":"コメント（日本語1〜2文）"}],"totalScore":合計(数値),"maxTotal":満点合計(数値),"overallComment":"総評（日本語2〜3文）"}`;
    try {
      const json = await callClaude(prompt);
      return JSON.parse(json);
    } catch (e) {
      alert("採点エラーが発生しました。再試行してください。");
      return null;
    } finally {
      setLoading(false);
    }
  };

  const submitExam = async () => {
    const unanswered = problems.filter((p) => !answers[p.id]?.trim());
    if (unanswered.length > 0) { alert(`${unanswered.length}問が未回答です。`); return; }
    const r = await gradeProblems(problems, answers, "AI採点中...");
    if (!r) return;
    setResults(r);
    const session = { id: Date.now(), studentName, level, problems, answers: { ...answers }, results: r, timestamp: new Date(), retryResults: null };
    setHistory((h) => [session, ...h]);
    setRetryResults(null);
    setSimilarProblems([]);
    setSimilarResults(null);
    setScreen("results");
    scrollTop();
  };

  const startRetry = () => {
    const wrongIds = results.results.filter((r) => r.status !== "correct").map((r) => r.id);
    setRetryProblems(problems.filter((p) => wrongIds.includes(p.id)));
    setRetryAnswers({});
    setRetryResults(null);
    setScreen("retry");
    scrollTop();
  };

  const submitRetry = async () => {
    const unanswered = retryProblems.filter((p) => !retryAnswers[p.id]?.trim());
    if (unanswered.length > 0) { alert(`${unanswered.length}問が未回答です。`); return; }
    const r = await gradeProblems(retryProblems, retryAnswers, "間違い直しを採点中...");
    if (!r) return;
    setRetryResults(r);
    setHistory((h) => h.map((s, i) => i === 0 ? { ...s, retryResults: r } : s));
    scrollTop();
  };

  const generateSimilar = async () => {
    const wrongList = results.results.filter((r) => r.status !== "correct");
    const wrongProbs = problems.filter((p) => wrongList.some((r) => r.id === p.id));
    setLoading(true);
    setLoadingMsg("類似問題を生成中...");
    const prompt = `数学の先生として、以下の問題それぞれに対して類似した別の問題を1問ずつ生成してください。難易度・形式は同じにしてください。\n\n${wrongProbs.map((p, i) => `問${i + 1}(id:${p.id}): ${p.question}`).join("\n")}\n\n以下のJSON形式のみで返してください（説明文・マークダウン記号なし）:\n{"problems":[{"originalId":元のid(数値),"question":"新しい問題文","points":点数(数値),"topic":"トピック"}]}`;
    try {
      const json = await callClaude(prompt);
      const parsed = JSON.parse(json);
      const newProbs = parsed.problems.map((p, i) => ({ ...p, id: 9000 + i }));
      setSimilarProblems(newProbs);
      setSimilarAnswers({});
      setSimilarResults(null);
      setScreen("similar");
    } catch (e) {
      alert("類似問題の生成に失敗しました。");
    } finally {
      setLoading(false);
    }
    scrollTop();
  };

  const submitSimilar = async () => {
    const unanswered = similarProblems.filter((p) => !similarAnswers[p.id]?.trim());
    if (unanswered.length > 0) { alert(`${unanswered.length}問が未回答です。`); return; }
    const r = await gradeProblems(similarProblems, similarAnswers, "類似問題を採点中...");
    if (!r) return;
    setSimilarResults(r);
    scrollTop();
  };

  const startExam = (lv) => {
    setLevel(lv);
    setProblems(PROBLEM_BANK[lv]);
    setAnswers({});
    setResults(null);
    setRetryResults(null);
    setSimilarProblems([]);
    setSimilarResults(null);
    setScreen("exam");
    scrollTop();
  };

  // ─── STYLES ───────────────────────────────────────────
  const css = `
    @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@300;400;500;700&family=Space+Mono:wght@400;700&display=swap');
    * { box-sizing: border-box; margin: 0; padding: 0; }
    ::-webkit-scrollbar { width: 4px; } ::-webkit-scrollbar-track { background: #0d0d18; } ::-webkit-scrollbar-thumb { background: #2a2a40; border-radius: 2px; }
    .card { background: #111120; border: 1px solid #1e1e32; border-radius: 14px; padding: 20px; margin-bottom: 14px; transition: border-color .2s; }
    .card:hover { border-color: #32324e; }
    .ai { width: 100%; background: #0a0a14; border: 1px solid #1e1e32; border-radius: 8px; padding: 10px 14px; color: #e2e8f0; font-family: 'Space Mono', monospace; font-size: 14px; outline: none; transition: border-color .2s; resize: none; }
    .ai:focus { border-color: #7c3aed; }
    .btn { border: none; border-radius: 10px; color: #fff; padding: 11px 22px; font-family: inherit; font-size: 14px; font-weight: 700; cursor: pointer; transition: all .2s; }
    .btn:disabled { opacity: .4; cursor: not-allowed; }
    .btn-p { background: linear-gradient(135deg,#7c3aed,#5b21b6); }
    .btn-p:hover:not(:disabled) { transform: translateY(-1px); box-shadow: 0 8px 24px rgba(124,58,237,.4); }
    .btn-a { background: linear-gradient(135deg,#d97706,#92400e); }
    .btn-a:hover:not(:disabled) { transform: translateY(-1px); box-shadow: 0 8px 24px rgba(217,119,6,.3); }
    .btn-g { background: linear-gradient(135deg,#16a34a,#065f46); }
    .btn-g:hover:not(:disabled) { transform: translateY(-1px); box-shadow: 0 8px 24px rgba(22,163,74,.3); }
    .btn-ghost { background: none; border: 1px solid #252538; color: #7c7c9e; border-radius: 8px; padding: 8px 16px; cursor: pointer; font-family: inherit; font-size: 13px; transition: all .2s; }
    .btn-ghost:hover { border-color: #3e3e5c; color: #a0aec0; }
    .badge { display: inline-block; padding: 3px 10px; border-radius: 20px; font-size: 12px; font-weight: 700; }
    .rcard { background: #111120; border-radius: 12px; padding: 18px; margin-bottom: 12px; border-left: 4px solid; }
    .nav-btn { background: none; border: none; cursor: pointer; padding: 10px 18px; font-family: inherit; font-size: 13px; font-weight: 500; border-bottom: 2px solid transparent; transition: all .2s; color: #3a3a56; white-space: nowrap; }
    .nav-btn.on { color: #a78bfa; border-bottom-color: #a78bfa; }
    .nav-btn:hover:not(.on) { color: #6464a0; }
    @keyframes fu { from{opacity:0;transform:translateY(10px)} to{opacity:1;transform:translateY(0)} }
    .fu { animation: fu .3s ease forwards; }
    @keyframes spin { to{transform:rotate(360deg)} }
    .lv-card { background: #111120; border: 2px solid #1e1e32; border-radius: 16px; padding: 22px; cursor: pointer; transition: all .25s; flex: 1; min-width: 160px; }
    .lv-card:hover { transform: translateY(-4px); }
    .hist-row { background: #111120; border: 1px solid #1e1e32; border-radius: 10px; padding: 14px 18px; margin-bottom: 10px; cursor: pointer; transition: border-color .2s; display: flex; align-items: center; justify-content: space-between; }
    .hist-row:hover { border-color: #3e3e5c; }
    .mono { font-family: 'Space Mono', monospace; }
  `;

  const SC = STATUS_COLOR;
  const wrongList = results ? results.results.filter((r) => r.status !== "correct") : [];

  return (
    <div ref={topRef} style={{ minHeight: "100vh", background: "#080812", fontFamily: "'Noto Sans JP', sans-serif", color: "#dde4f0" }}>
      <style>{css}</style>

      {/* HEADER */}
      <div style={{ background: "#0a0a16", borderBottom: "1px solid #16162a", padding: "14px 20px", position: "sticky", top: 0, zIndex: 20 }}>
        <div style={{ maxWidth: 820, margin: "0 auto", display: "flex", alignItems: "center", justifyContent: "space-between", gap: 8 }}>
          <div style={{ display: "flex", alignItems: "center", gap: 10, cursor: "pointer" }} onClick={() => setScreen("home")}>
            <div style={{ width: 34, height: 34, background: "linear-gradient(135deg,#7c3aed,#4f46e5)", borderRadius: 9, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 17 }}>∑</div>
            <div>
              <div className="mono" style={{ fontWeight: 700, fontSize: 16 }}>AutoGrade</div>
              <div style={{ fontSize: 10, color: "#3a3a56" }}>AI 数学自動採点</div>
            </div>
          </div>
          <div style={{ display: "flex", overflowX: "auto" }}>
            {[
              { s: "home", label: "🏠 ホーム", show: true },
              { s: "exam", label: "📝 試験", show: screen !== "home" },
              { s: "results", label: "📊 結果", show: !!results },
              { s: "retry", label: "🔁 直し", show: !!results && wrongList.length > 0 },
              { s: "similar", label: "🎯 類似", show: similarProblems.length > 0 },
              { s: "history", label: "📚 履歴", show: true },
            ].filter((x) => x.show).map(({ s, label }) => (
              <button key={s} className={`nav-btn ${screen === s ? "on" : ""}`} onClick={() => setScreen(s)}>{label}</button>
            ))}
          </div>
        </div>
      </div>

      {/* LOADING */}
      {loading && (
        <div style={{ position: "fixed", inset: 0, background: "rgba(8,8,18,.88)", zIndex: 100, display: "flex", flexDirection: "column", alignItems: "center", justifyContent: "center", gap: 18 }}>
          <div style={{ width: 50, height: 50, border: "3px solid rgba(124,58,237,.25)", borderTopColor: "#7c3aed", borderRadius: "50%", animation: "spin .75s linear infinite" }} />
          <p className="mono" style={{ color: "#a78bfa", fontSize: 14 }}>{loadingMsg}</p>
        </div>
      )}

      <div style={{ maxWidth: 820, margin: "0 auto", padding: "24px 14px" }}>

        {/* ══ HOME ══ */}
        {screen === "home" && (
          <div className="fu">
            <div style={{ textAlign: "center", marginBottom: 32 }}>
              <p style={{ fontSize: 12, color: "#3a3a56", marginBottom: 4 }}>レベルを選んで試験を開始</p>
              <h1 className="mono" style={{ fontSize: 26, fontWeight: 700 }}>どのレベルで挑戦しますか？</h1>
            </div>
            <div style={{ marginBottom: 24, maxWidth: 320 }}>
              <label style={{ fontSize: 12, color: "#5a5a7e", display: "block", marginBottom: 6 }}>生徒名（任意）</label>
              <input className="ai" style={{ borderRadius: 8 }} placeholder="例：山田 太郎" value={studentName} onChange={(e) => setStudentName(e.target.value)} />
            </div>
            <div style={{ display: "flex", gap: 14, flexWrap: "wrap", marginBottom: 40 }}>
              {Object.entries(LEVEL_META).map(([lv, m]) => {
                const probs = PROBLEM_BANK[lv];
                const pts = probs.reduce((s, p) => s + p.points, 0);
                return (
                  <div key={lv} className="lv-card" style={{ borderColor: m.border }} onClick={() => startExam(lv)}>
                    <div style={{ fontSize: 26, marginBottom: 10 }}>{m.emoji}</div>
                    <div style={{ fontWeight: 700, fontSize: 15, color: m.color, marginBottom: 4 }}>{m.label}</div>
                    <div style={{ fontSize: 11, color: "#5a5a7e", marginBottom: 10 }}>{probs.length}問 / {pts}点満点</div>
                    <div style={{ display: "flex", flexWrap: "wrap", gap: 4 }}>
                      {probs.map((p) => <span key={p.id} style={{ background: m.bg, color: m.color, fontSize: 10, padding: "2px 7px", borderRadius: 4 }}>{p.topic}</span>)}
                    </div>
                  </div>
                );
              })}
            </div>
            {history.length > 0 && (
              <>
                <div style={{ display: "flex", alignItems: "center", justifyContent: "space-between", marginBottom: 12 }}>
                  <p style={{ fontSize: 13, fontWeight: 600, color: "#7c7ca0" }}>最近の履歴</p>
                  <button className="btn-ghost" onClick={() => setScreen("history")}>すべて見る →</button>
                </div>
                {history.slice(0, 3).map((s) => {
                  const pct = Math.round((s.results.totalScore / s.results.maxTotal) * 100);
                  const g = getGrade(pct);
                  const m = LEVEL_META[s.level];
                  return (
                    <div key={s.id} className="hist-row" onClick={() => { setHistoryDetail(s); setScreen("history"); }}>
                      <div style={{ display: "flex", alignItems: "center", gap: 12 }}>
                        <span style={{ fontSize: 20 }}>{m.emoji}</span>
                        <div>
                          <p style={{ fontSize: 13, fontWeight: 600 }}>{s.studentName || "名前なし"} — {m.label}</p>
                          <p style={{ fontSize: 11, color: "#3a3a56" }}>{s.timestamp.toLocaleString("ja-JP")}</p>
                        </div>
                      </div>
                      <div style={{ textAlign: "right" }}>
                        <span className="mono" style={{ fontSize: 20, fontWeight: 700, color: g.color }}>{g.label}</span>
                        <p style={{ fontSize: 11, color: "#5a5a7e" }}>{pct}%</p>
                      </div>
                    </div>
                  );
                })}
              </>
            )}
          </div>
        )}

        {/* ══ EXAM ══ */}
        {screen === "exam" && (
          <div className="fu">
            <div style={{ display: "flex", alignItems: "center", gap: 12, marginBottom: 20 }}>
              <span style={{ fontSize: 22 }}>{LEVEL_META[level].emoji}</span>
              <div>
                <p style={{ fontSize: 13, color: LEVEL_META[level].color, fontWeight: 600 }}>{LEVEL_META[level].label}レベル</p>
                <p style={{ fontSize: 11, color: "#3a3a56" }}>全{problems.length}問 / {problems.reduce((s, p) => s + p.points, 0)}点満点</p>
              </div>
              {studentName && <span style={{ marginLeft: "auto", fontSize: 13, color: "#5a5a7e" }}>{studentName}</span>}
            </div>
            {problems.map((p, i) => {
              const m = LEVEL_META[level];
              return (
                <div key={p.id} className="card" style={{ borderColor: answers[p.id]?.trim() ? m.border : "#1e1e32" }}>
                  <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 10 }}>
                    <div style={{ display: "flex", gap: 7, alignItems: "center" }}>
                      <span className="mono" style={{ background: "#1a1a30", color: "#a78bfa", fontSize: 11, fontWeight: 700, padding: "3px 8px", borderRadius: 6 }}>問{i + 1}</span>
                      <span style={{ background: m.bg, color: m.color, fontSize: 10, padding: "2px 7px", borderRadius: 4 }}>{p.topic}</span>
                      <span style={{ fontSize: 11, color: "#3a3a56" }}>{p.points}点</span>
                    </div>
                    {answers[p.id]?.trim() && <span style={{ color: "#22c55e", fontSize: 16 }}>✓</span>}
                  </div>
                  <p style={{ fontSize: 15, lineHeight: 1.8, marginBottom: 12, color: "#c8d4e8" }}>{p.question}</p>
                  <textarea className="ai" rows={2} placeholder="解答を入力..." value={answers[p.id] || ""} onChange={(e) => setAnswers((a) => ({ ...a, [p.id]: e.target.value }))} />
                </div>
              );
            })}
            <div style={{ marginTop: 24, textAlign: "center" }}>
              <button className="btn btn-p" onClick={submitExam} disabled={loading}>🎯 採点する</button>
            </div>
          </div>
        )}

        {/* ══ RESULTS ══ */}
        {screen === "results" && results && (() => {
          const pct = Math.round((results.totalScore / results.maxTotal) * 100);
          const g = getGrade(pct);
          return (
            <div className="fu">
              <div style={{ background: "linear-gradient(135deg,#10102a,#0c0c20)", border: "1px solid #1e1e38", borderRadius: 18, padding: 26, marginBottom: 22, textAlign: "center" }}>
                {studentName && <p style={{ fontSize: 12, color: "#3a3a56", marginBottom: 6 }}>{studentName} さんの成績 — {LEVEL_META[level].label}レベル</p>}
                <div style={{ display: "flex", justifyContent: "center", alignItems: "center", gap: 32, flexWrap: "wrap" }}>
                  {[
                    { v: g.label, s: "グレード" },
                    { v: `${results.totalScore}/${results.maxTotal}`, s: "得点" },
                    { v: `${pct}%`, s: "正答率" },
                    { v: `${wrongList.length}問`, s: "要復習", c: wrongList.length > 0 ? "#ef4444" : "#22c55e" },
                  ].map((x, i) => (
                    <div key={i} style={{ textAlign: "center" }}>
                      <div className="mono" style={{ fontSize: 36, fontWeight: 700, color: x.c || g.color, lineHeight: 1 }}>{x.v}</div>
                      <div style={{ fontSize: 11, color: "#3a3a56", marginTop: 4 }}>{x.s}</div>
                    </div>
                  ))}
                </div>
                <div style={{ marginTop: 16, background: "#080812", borderRadius: 8, padding: "12px 16px" }}>
                  <p style={{ fontSize: 13, color: "#8a96b0", lineHeight: 1.7 }}>{results.overallComment}</p>
                </div>
              </div>
              {wrongList.length > 0 && (
                <div style={{ display: "flex", gap: 10, flexWrap: "wrap", marginBottom: 22 }}>
                  <button className="btn btn-a" onClick={startRetry}>🔁 間違い直し（{wrongList.length}問）</button>
                  <button className="btn btn-g" onClick={generateSimilar} disabled={loading}>🎯 類似問題に挑戦</button>
                </div>
              )}
              {results.results.map((r, i) => {
                const p = problems.find((x) => x.id === r.id);
                const sc = SC[r.status] || "#94a3b8";
                return (
                  <div key={r.id} className="rcard" style={{ borderLeftColor: sc }}>
                    <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 8 }}>
                      <div style={{ display: "flex", gap: 8, alignItems: "center" }}>
                        <span className="mono badge" style={{ background: "#1a1a30", color: "#a78bfa", fontSize: 11, fontWeight: 700 }}>問{i + 1}</span>
                        <span className="badge" style={{ background: `${sc}18`, color: sc }}>{STATUS_LABEL[r.status]}</span>
                      </div>
                      <span className="mono" style={{ fontSize: 14, fontWeight: 700, color: sc }}>{r.score}<span style={{ fontSize: 11, color: "#3a3a56" }}>/{r.maxScore}点</span></span>
                    </div>
                    <p style={{ fontSize: 13, color: "#5a5a7e", marginBottom: 8 }}>{p?.question}</p>
                    <div style={{ background: "#080812", borderRadius: 7, padding: "8px 12px", marginBottom: 6 }}>
                      <span style={{ fontSize: 11, color: "#3a3a56" }}>解答：</span>
                      <span className="mono" style={{ fontSize: 13, color: "#dde4f0", marginLeft: 6 }}>{answers[r.id]}</span>
                    </div>
                    {r.status !== "correct" && (
                      <div style={{ background: "#0a1a10", borderRadius: 7, padding: "8px 12px", marginBottom: 6 }}>
                        <span style={{ fontSize: 11, color: "#3a3a56" }}>正解：</span>
                        <span className="mono" style={{ fontSize: 13, color: "#22c55e", marginLeft: 6 }}>{r.correctAnswer}</span>
                      </div>
                    )}
                    <p style={{ fontSize: 13, color: "#8a96b0", lineHeight: 1.6 }}>💬 {r.feedback}</p>
                  </div>
                );
              })}
            </div>
          );
        })()}

        {/* ══ RETRY ══ */}
        {screen === "retry" && (
          <div className="fu">
            <div style={{ marginBottom: 20 }}>
              <p style={{ color: "#f59e0b", fontSize: 13, fontWeight: 600, marginBottom: 3 }}>🔁 間違い直し</p>
              <p style={{ fontSize: 12, color: "#3a3a56" }}>前回間違えた {retryProblems.length} 問に再挑戦しましょう</p>
            </div>
            {!retryResults ? (
              <>
                {retryProblems.map((p, i) => (
                  <div key={p.id} className="card" style={{ borderColor: retryAnswers[p.id]?.trim() ? "#3a2800" : "#1e1e32", borderLeftWidth: 3, borderLeftColor: "#f59e0b" }}>
                    <div style={{ display: "flex", gap: 8, alignItems: "center", marginBottom: 10 }}>
                      <span className="mono badge" style={{ background: "#1f1400", color: "#f59e0b", fontSize: 11, fontWeight: 700 }}>問{i + 1}</span>
                      <span style={{ background: "#1f1400", color: "#f59e0b", fontSize: 10, padding: "2px 7px", borderRadius: 4 }}>{p.topic}</span>
                    </div>
                    <p style={{ fontSize: 15, lineHeight: 1.8, marginBottom: 12, color: "#c8d4e8" }}>{p.question}</p>
                    <textarea className="ai" rows={2} placeholder="解答を入力..." value={retryAnswers[p.id] || ""} onChange={(e) => setRetryAnswers((a) => ({ ...a, [p.id]: e.target.value }))} />
                  </div>
                ))}
                <div style={{ marginTop: 22, display: "flex", gap: 10, justifyContent: "center" }}>
                  <button className="btn-ghost" onClick={() => setScreen("results")}>← 結果に戻る</button>
                  <button className="btn btn-a" onClick={submitRetry} disabled={loading}>🎯 採点する</button>
                </div>
              </>
            ) : (
              <div>
                <div style={{ background: "#12100a", border: "1px solid #2a2000", borderRadius: 14, padding: 20, marginBottom: 20, textAlign: "center" }}>
                  <p style={{ fontSize: 13, color: "#f59e0b", marginBottom: 8 }}>間違い直し結果</p>
                  <div className="mono" style={{ fontSize: 34, fontWeight: 700, color: "#f59e0b" }}>{retryResults.totalScore}<span style={{ fontSize: 16, color: "#3a3a56" }}>/{retryResults.maxTotal}点</span></div>
                  <p style={{ fontSize: 13, color: "#8a96b0", marginTop: 12 }}>{retryResults.overallComment}</p>
                </div>
                {retryResults.results.map((r, i) => {
                  const p = retryProblems.find((x) => x.id === r.id);
                  const sc = SC[r.status] || "#94a3b8";
                  return (
                    <div key={r.id} className="rcard" style={{ borderLeftColor: sc }}>
                      <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 7 }}>
                        <div style={{ display: "flex", gap: 8 }}>
                          <span className="mono badge" style={{ background: "#1a1a30", color: "#a78bfa", fontSize: 11, fontWeight: 700 }}>問{i + 1}</span>
                          <span className="badge" style={{ background: `${sc}18`, color: sc }}>{STATUS_LABEL[r.status]}</span>
                        </div>
                        <span className="mono" style={{ fontSize: 13, color: sc }}>{r.score}/{r.maxScore}点</span>
                      </div>
                      <p style={{ fontSize: 13, color: "#5a5a7e", marginBottom: 7 }}>{p?.question}</p>
                      <div style={{ background: "#080812", borderRadius: 7, padding: "8px 12px", marginBottom: 6 }}>
                        <span style={{ fontSize: 11, color: "#3a3a56" }}>解答：</span>
                        <span className="mono" style={{ fontSize: 13, color: "#dde4f0", marginLeft: 6 }}>{retryAnswers[r.id]}</span>
                      </div>
                      {r.status !== "correct" && (
                        <div style={{ background: "#0a1a10", borderRadius: 7, padding: "8px 12px", marginBottom: 6 }}>
                          <span style={{ fontSize: 11, color: "#3a3a56" }}>正解：</span>
                          <span className="mono" style={{ fontSize: 13, color: "#22c55e", marginLeft: 6 }}>{r.correctAnswer}</span>
                        </div>
                      )}
                      <p style={{ fontSize: 13, color: "#8a96b0" }}>💬 {r.feedback}</p>
                    </div>
                  );
                })}
                <div style={{ marginTop: 20, display: "flex", gap: 10, justifyContent: "center" }}>
                  <button className="btn-ghost" onClick={() => setScreen("results")}>← 元の結果へ</button>
                  <button className="btn btn-g" onClick={() => { setRetryResults(null); setRetryAnswers({}); }}>もう一度やり直す</button>
                </div>
              </div>
            )}
          </div>
        )}

        {/* ══ SIMILAR ══ */}
        {screen === "similar" && (
          <div className="fu">
            <div style={{ marginBottom: 20 }}>
              <p style={{ color: "#22c55e", fontSize: 13, fontWeight: 600, marginBottom: 3 }}>🎯 類似問題チャレンジ</p>
              <p style={{ fontSize: 12, color: "#3a3a56" }}>間違えた問題と同じ形式の新しい問題です。AIが生成しました。</p>
            </div>
            {!similarResults ? (
              <>
                {similarProblems.map((p, i) => (
                  <div key={p.id} className="card" style={{ borderColor: similarAnswers[p.id]?.trim() ? "#0a1a10" : "#1e1e32", borderLeftWidth: 3, borderLeftColor: "#22c55e" }}>
                    <div style={{ display: "flex", gap: 8, alignItems: "center", marginBottom: 10 }}>
                      <span className="mono badge" style={{ background: "#0a1a10", color: "#22c55e", fontSize: 11, fontWeight: 700 }}>問{i + 1}</span>
                      {p.topic && <span style={{ background: "#0a1a10", color: "#22c55e", fontSize: 10, padding: "2px 7px", borderRadius: 4 }}>{p.topic}</span>}
                    </div>
                    <p style={{ fontSize: 15, lineHeight: 1.8, marginBottom: 12, color: "#c8d4e8" }}>{p.question}</p>
                    <textarea className="ai" rows={2} placeholder="解答を入力..." value={similarAnswers[p.id] || ""} onChange={(e) => setSimilarAnswers((a) => ({ ...a, [p.id]: e.target.value }))} />
                  </div>
                ))}
                <div style={{ marginTop: 22, display: "flex", gap: 10, justifyContent: "center" }}>
                  <button className="btn-ghost" onClick={() => setScreen("results")}>← 結果に戻る</button>
                  <button className="btn btn-g" onClick={submitSimilar} disabled={loading}>🎯 採点する</button>
                </div>
              </>
            ) : (
              <div>
                <div style={{ background: "#0a1a10", border: "1px solid #1a3a20", borderRadius: 14, padding: 20, marginBottom: 20, textAlign: "center" }}>
                  <p style={{ fontSize: 13, color: "#22c55e", marginBottom: 8 }}>類似問題 結果</p>
                  <div className="mono" style={{ fontSize: 34, fontWeight: 700, color: "#22c55e" }}>{similarResults.totalScore}<span style={{ fontSize: 16, color: "#3a3a56" }}>/{similarResults.maxTotal}点</span></div>
                  <p style={{ fontSize: 13, color: "#8a96b0", marginTop: 12 }}>{similarResults.overallComment}</p>
                </div>
                {similarResults.results.map((r, i) => {
                  const p = similarProblems.find((x) => x.id === r.id);
                  const sc = SC[r.status] || "#94a3b8";
                  return (
                    <div key={r.id} className="rcard" style={{ borderLeftColor: sc }}>
                      <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 7 }}>
                        <div style={{ display: "flex", gap: 8 }}>
                          <span className="mono badge" style={{ background: "#1a1a30", color: "#a78bfa", fontSize: 11, fontWeight: 700 }}>問{i + 1}</span>
                          <span className="badge" style={{ background: `${sc}18`, color: sc }}>{STATUS_LABEL[r.status]}</span>
                        </div>
                        <span className="mono" style={{ fontSize: 13, color: sc }}>{r.score}/{r.maxScore}点</span>
                      </div>
                      <p style={{ fontSize: 13, color: "#5a5a7e", marginBottom: 7 }}>{p?.question}</p>
                      <div style={{ background: "#080812", borderRadius: 7, padding: "8px 12px", marginBottom: 6 }}>
                        <span style={{ fontSize: 11, color: "#3a3a56" }}>解答：</span>
                        <span className="mono" style={{ fontSize: 13, color: "#dde4f0", marginLeft: 6 }}>{similarAnswers[r.id]}</span>
                      </div>
                      {r.status !== "correct" && (
                        <div style={{ background: "#0a1a10", borderRadius: 7, padding: "8px 12px", marginBottom: 6 }}>
                          <span style={{ fontSize: 11, color: "#3a3a56" }}>正解：</span>
                          <span className="mono" style={{ fontSize: 13, color: "#22c55e", marginLeft: 6 }}>{r.correctAnswer}</span>
                        </div>
                      )}
                      <p style={{ fontSize: 13, color: "#8a96b0" }}>💬 {r.feedback}</p>
                    </div>
                  );
                })}
                <div style={{ marginTop: 20, display: "flex", gap: 10, justifyContent: "center" }}>
                  <button className="btn-ghost" onClick={() => setScreen("results")}>← 結果へ戻る</button>
                  <button className="btn btn-g" onClick={() => { setSimilarResults(null); setSimilarAnswers({}); }}>もう一度やり直す</button>
                </div>
              </div>
            )}
          </div>
        )}

        {/* ══ HISTORY ══ */}
        {screen === "history" && (
          <div className="fu">
            <div style={{ display: "flex", alignItems: "center", justifyContent: "space-between", marginBottom: 22 }}>
              <div>
                <p style={{ color: "#a78bfa", fontSize: 13, fontWeight: 600 }}>📚 受験履歴</p>
                <p style={{ fontSize: 12, color: "#3a3a56" }}>{history.length}件の記録</p>
              </div>
              {historyDetail && <button className="btn-ghost" onClick={() => setHistoryDetail(null)}>← 一覧に戻る</button>}
            </div>
            {history.length === 0 ? (
              <div style={{ textAlign: "center", padding: "60px 20px", color: "#3a3a56" }}>
                <p style={{ fontSize: 32, marginBottom: 12 }}>📭</p>
                <p>まだ履歴がありません</p>
              </div>
            ) : !historyDetail ? (
              history.map((s) => {
                const pct = Math.round((s.results.totalScore / s.results.maxTotal) * 100);
                const g = getGrade(pct);
                const m = LEVEL_META[s.level];
                const wc = s.results.results.filter((r) => r.status !== "correct").length;
                return (
                  <div key={s.id} className="hist-row" onClick={() => setHistoryDetail(s)}>
                    <div style={{ display: "flex", alignItems: "center", gap: 12 }}>
                      <div style={{ width: 40, height: 40, background: m.bg, border: `1px solid ${m.border}`, borderRadius: 10, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 18 }}>{m.emoji}</div>
                      <div>
                        <p style={{ fontSize: 13, fontWeight: 600 }}>{s.studentName || "名前なし"}</p>
                        <div style={{ display: "flex", gap: 6, alignItems: "center", marginTop: 3, flexWrap: "wrap" }}>
                          <span style={{ fontSize: 10, color: m.color, background: m.bg, padding: "1px 6px", borderRadius: 4 }}>{m.label}</span>
                          {wc > 0 && <span style={{ fontSize: 11, color: "#ef4444" }}>✗{wc}問</span>}
                          {s.retryResults && <span style={{ fontSize: 11, color: "#22c55e" }}>🔁直し済</span>}
                          <span style={{ fontSize: 11, color: "#3a3a56" }}>{s.timestamp.toLocaleString("ja-JP")}</span>
                        </div>
                      </div>
                    </div>
                    <div style={{ textAlign: "right" }}>
                      <span className="mono" style={{ fontSize: 20, fontWeight: 700, color: g.color }}>{g.label}</span>
                      <p style={{ fontSize: 11, color: "#5a5a7e" }}>{s.results.totalScore}/{s.results.maxTotal} ({pct}%)</p>
                    </div>
                  </div>
                );
              })
            ) : (
              (() => {
                const s = historyDetail;
                const pct = Math.round((s.results.totalScore / s.results.maxTotal) * 100);
                const g = getGrade(pct);
                const m = LEVEL_META[s.level];
                return (
                  <>
                    <div style={{ background: "#10102a", border: "1px solid #1e1e38", borderRadius: 14, padding: 20, marginBottom: 20, textAlign: "center" }}>
                      <p style={{ fontSize: 12, color: "#3a3a56", marginBottom: 6 }}>{s.studentName || "名前なし"} — {m.emoji} {m.label} — {s.timestamp.toLocaleString("ja-JP")}</p>
                      <div className="mono" style={{ fontSize: 38, fontWeight: 700, color: g.color }}>{g.label}</div>
                      <div className="mono" style={{ fontSize: 22, marginTop: 4 }}>{s.results.totalScore}/{s.results.maxTotal}点 <span style={{ fontSize: 13, color: "#3a3a56" }}>({pct}%)</span></div>
                      <p style={{ fontSize: 13, color: "#8a96b0", marginTop: 10 }}>{s.results.overallComment}</p>
                    </div>
                    {s.results.results.map((r, i) => {
                      const p = s.problems.find((x) => x.id === r.id);
                      const sc = SC[r.status] || "#94a3b8";
                      return (
                        <div key={r.id} className="rcard" style={{ borderLeftColor: sc }}>
                          <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 7 }}>
                            <div style={{ display: "flex", gap: 8 }}>
                              <span className="mono badge" style={{ background: "#1a1a30", color: "#a78bfa", fontSize: 11, fontWeight: 700 }}>問{i + 1}</span>
                              <span className="badge" style={{ background: `${sc}18`, color: sc }}>{STATUS_LABEL[r.status]}</span>
                            </div>
                            <span className="mono" style={{ fontSize: 13, color: sc }}>{r.score}/{r.maxScore}点</span>
                          </div>
                          <p style={{ fontSize: 13, color: "#5a5a7e", marginBottom: 7 }}>{p?.question}</p>
                          <div style={{ background: "#080812", borderRadius: 7, padding: "8px 12px" }}>
                            <span style={{ fontSize: 11, color: "#3a3a56" }}>解答：</span>
                            <span className="mono" style={{ fontSize: 13, color: "#dde4f0", marginLeft: 6 }}>{s.answers[r.id]}</span>
                          </div>
                        </div>
                      );
                    })}
                    {s.retryResults && (
                      <div style={{ marginTop: 18, background: "#12100a", border: "1px solid #2a2000", borderRadius: 14, padding: 16 }}>
                        <p style={{ color: "#f59e0b", fontSize: 13, fontWeight: 600, marginBottom: 8 }}>🔁 間違い直し結果</p>
                        <div className="mono" style={{ fontSize: 22, color: "#f59e0b" }}>{s.retryResults.totalScore}/{s.retryResults.maxTotal}点</div>
                        <p style={{ fontSize: 13, color: "#8a96b0", marginTop: 8 }}>{s.retryResults.overallComment}</p>
                      </div>
                    )}
                  </>
                );
              })()
            )}
          </div>
        )}

      </div>
    </div>
  );
}
