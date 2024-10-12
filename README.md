import React, { useState, useEffect } from 'react';
import { shuffle } from 'lodash';

const terms = [
  { english: "Photosynthesis", arabic: "التمثيل الضوئي" },
  { english: "Semiconductor", arabic: "شبه موصل" },
  { english: "Algorithm", arabic: "خوارزمية" },
  { english: "Biodiversity", arabic: "التنوع البيولوجي" },
  { english: "Nanotechnology", arabic: "تقنية النانو" },
  { english: "Quantum computing", arabic: "الحوسبة الكمية" },
  { english: "Genome", arabic: "الجينوم" },
  { english: "Artificial intelligence", arabic: "الذكاء الاصطناعي" },
  { english: "Renewable energy", arabic: "الطاقة المتجددة" },
  { english: "Ecosystem", arabic: "النظام البيئي" }
];

const TermMatchingGame = () => {
  const [englishTerms, setEnglishTerms] = useState([]);
  const [arabicTerms, setArabicTerms] = useState([]);
  const [selectedEnglish, setSelectedEnglish] = useState(null);
  const [selectedArabic, setSelectedArabic] = useState(null);
  const [matchedPairs, setMatchedPairs] = useState([]);

  useEffect(() => {
    setEnglishTerms(shuffle(terms.map(term => term.english)));
    setArabicTerms(shuffle(terms.map(term => term.arabic)));
  }, []);

  useEffect(() => {
    if (selectedEnglish && selectedArabic) {
      const englishTerm = terms.find(term => term.english === selectedEnglish);
      if (englishTerm && englishTerm.arabic === selectedArabic) {
        setMatchedPairs([...matchedPairs, englishTerm]);
        setEnglishTerms(englishTerms.filter(term => term !== selectedEnglish));
        setArabicTerms(arabicTerms.filter(term => term !== selectedArabic));
      }
      setSelectedEnglish(null);
      setSelectedArabic(null);
    }
  }, [selectedEnglish, selectedArabic]);

  return (
    <div className="p-4">
      <h2 className="text-xl font-bold mb-4">Match the technical terms</h2>
      <div className="flex justify-between">
        <div className="w-1/2 pr-2">
          <h3 className="font-semibold mb-2">English Terms</h3>
          {englishTerms.map(term => (
            <button
              key={term}
              onClick={() => setSelectedEnglish(term)}
              className={`block w-full text-left p-2 mb-2 rounded ${
                selectedEnglish === term ? 'bg-blue-500 text-white' : 'bg-gray-200'
              }`}
            >
              {term}
            </button>
          ))}
        </div>
        <div className="w-1/2 pl-2">
          <h3 className="font-semibold mb-2">Arabic Terms</h3>
          {arabicTerms.map(term => (
            <button
              key={term}
              onClick={() => setSelectedArabic(term)}
              className={`block w-full text-right p-2 mb-2 rounded ${
                selectedArabic === term ? 'bg-blue-500 text-white' : 'bg-gray-200'
              }`}
            >
              {term}
            </button>
          ))}
        </div>
      </div>
      <div className="mt-4">
        <h3 className="font-semibold mb-2">Matched Pairs</h3>
        {matchedPairs.map(pair => (
          <div key={pair.english} className="flex justify-between bg-green-100 p-2 mb-2 rounded">
            <span>{pair.english}</span>
            <span>{pair.arabic}</span>
          </div>
        ))}
      </div>
    </div>
  );
};

export default TermMatchingGame;
